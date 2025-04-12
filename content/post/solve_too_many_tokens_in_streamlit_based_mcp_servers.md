---
title: "Solve Too Many Tokens in Streamlit-Based MCP Servers"
date: 2025-04-11T21:25:30-07:00
categories:
  - Technology
tags:
  - MCP
  - MCP Server
  - MCP Client
  - Streamlit
  - FastAPI
  - Too Many Tokens
  - Token Limit
  - Context Window
  - Open AI
  - Claude
---

# Solving "Too Many Tokens" in Streamlit-Based MCP Servers: A Deep Dive

When you're building a Model Context Protocol (MCP) server using Streamlit, you may encounter the notorious error: **"Too many tokens"**. This occurs when the input passed to a large language model (LLM) exceeds the maximum token limit allowed by the API (e.g., OpenAI's GPT-4, Claude, etc.). It's a common bottleneck in systems that continuously build up context over time.

In this comprehensive post, we'll dive into why this error happens and how to fix it using practical strategies including summarization, context pruning, token counting, message prioritization, and more. We'll also include code snippets that you can use directly in your Streamlit-based MCP apps.

---

## Understanding the Error

Most LLMs have a **token limit** (e.g., 8K, 16K, 32K tokens). A token can be as small as a character or as long as a word. When you feed too much context into the model, the combined tokens of your prompt + context + system instructions + user input + expected output may exceed this limit.

In MCP systems, where each user interaction adds to a growing conversation history or set of memory artifacts, this is especially easy to hit.

---

## Fix #1: Token Counting and Monitoring

Before fixing the problem, measure it. Use libraries like `tiktoken` to count tokens.

### 📁 Example
```python
import tiktoken

def count_tokens(text, model="gpt-4"):
    enc = tiktoken.encoding_for_model(model)
    return len(enc.encode(text))
```

Use this to measure each message or the total context length.

### Streamlit Integration
```python
import streamlit as st

if 'messages' not in st.session_state:
    st.session_state['messages'] = []


<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- cpa -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-2843564932689995"
     data-ad-slot="3526097725"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# Count total tokens
total_tokens = sum(count_tokens(msg['content']) for msg in st.session_state['messages'])
st.write(f"Total tokens: {total_tokens}")
```

---

## Fix #2: Summarization of Prior Messages

If your chat history grows long, summarize previous messages and store them as a single concise message.

### ✍️ Manual Summarization Trigger
```python
def summarize_conversation(messages, model="gpt-3.5-turbo"):
    from openai import OpenAI
    client = OpenAI()

    content = "\n".join([f"{msg['role']}: {msg['content']}" for msg in messages])
    response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "system", "content": "Summarize the following conversation:"},
            {"role": "user", "content": content}
        ]
    )
    return response.choices[0].message.content
```

Use a button to summarize old messages:
```python
if st.button("Summarize conversation"):
    summary = summarize_conversation(st.session_state['messages'][:-5])  # keep recent messages
    st.session_state['messages'] = [{"role": "system", "content": summary}] + st.session_state['messages'][-5:]
```

---

## Fix #3: Context Pruning (Last-N Messages)

A fast fix: limit context to the **last N interactions**.

```python
MAX_MESSAGES = 10
context = st.session_state['messages'][-MAX_MESSAGES:]
```

If the total tokens still exceed the budget, fall back to more aggressive pruning or summarization.

---

## Fix #4: Message Prioritization

Not all messages are equally important. Prioritize based on:
- Recency
- Relevance (e.g., does it include an instruction?)
- Role (e.g., system > assistant > user)

### Prioritize Using Metadata
```python
def get_priority_score(msg):
    score = 0
    if msg['role'] == 'system': score += 2
    if 'important' in msg.get('metadata', {}): score += 2
    if msg['role'] == 'user': score += 1
    return score

pruned = sorted(st.session_state['messages'], key=get_priority_score, reverse=True)
context = pruned[:10]  # or as many as tokens allow
```

---

## Fix #5: Use External Memory Store

For long-term memory, move older context to an **external memory store** like a database or vector store. Retrieve relevant chunks using embeddings.

### 🔗 Example Using FAISS
```python
from sentence_transformers import SentenceTransformer
import faiss
import numpy as np

model = SentenceTransformer('all-MiniLM-L6-v2')
index = faiss.IndexFlatL2(384)  # 384 = embedding size for the model
texts = [msg['content'] for msg in st.session_state['messages']]
embeddings = model.encode(texts)
index.add(np.array(embeddings))

# Later, retrieve top-k similar context chunks
query_embedding = model.encode([user_input])[0]
D, I = index.search(np.array([query_embedding]), k=5)
relevant_texts = [texts[i] for i in I[0]]
```

---

## Fix #6: Chunking Long Text Inputs

If the user pastes a large document, chunk it before sending.

### 📝 Chunk Utility
```python
def chunk_text(text, max_tokens=500):
    words = text.split()
    chunks = []
    current = []
    for word in words:
        current.append(word)
        if count_tokens(" ".join(current)) > max_tokens:
            current.pop()
            chunks.append(" ".join(current))
            current = [word]
    if current:
        chunks.append(" ".join(current))
    return chunks
```

---

## Fix #7: Use Models With Larger Context Windows

Sometimes, the best fix is architectural. Switch to a model with a 32K or 128K token limit, like:
- GPT-4 Turbo (128K tokens)
- Claude 2/3
- Gemini 1.5 Pro (1M context)

Make sure your Streamlit code can dynamically select the model:
```python
model = st.selectbox("Choose a model", ["gpt-3.5-turbo", "gpt-4", "gpt-4-128k"])
```

---

## Bonus: Visualization for Debugging

Use Streamlit's visual tools to understand what's going into your model.
```python
with st.expander("View Raw Prompt"):
    st.code("\n".join([f"{m['role']}: {m['content']}" for m in context]))
```

Also display token budget:
```python
TOKEN_LIMIT = 8000
context_tokens = sum(count_tokens(m['content']) for m in context)
st.progress(context_tokens / TOKEN_LIMIT)
st.write(f"{context_tokens} / {TOKEN_LIMIT} tokens used")
```

---

## Wrap Up

Building an MCP server in Streamlit gives you powerful interactivity and rapid prototyping, but handling token overflow is a critical technical challenge. By implementing smart strategies like summarization, pruning, external memory, and better model choices, you can keep your system efficient and responsive.

Always start with visibility: track your tokens, inspect context, and allow debugging. Then apply the right combination of strategies based on your specific app's complexity, user behavior, and model capabilities.