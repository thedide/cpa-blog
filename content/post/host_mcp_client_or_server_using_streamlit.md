---
title: "Host MCP Client or Server Using Streamlit"
date: 2025-04-09T20:01:02-07:00
categories:
  - Technology
tags:
  - MCP
  - MCP Server
  - MCP Client
  - Streamlit
  - FastAPI
---

# Hosting a Model Context Protocol (MCP) Client or Server with Streamlit

As large language models (LLMs) continue to evolve, so too does the need for managing context efficiently and modularly. The **Model Context Protocol (MCP)** is emerging as a framework for managing, exchanging, and interpreting the context in which a model operates. Whether you're building applications that use Retrieval-Augmented Generation (RAG), integrating long-term memory, or developing interactive agents, a well-defined MCP architecture can make your system more scalable and interpretable.

In this blog post, we'll explore how to use **Streamlit**, a popular Python framework for building interactive web apps, to host an **MCP client or server**. We'll go step-by-step through setting up a basic MCP schema, building a Streamlit interface, and running it as a context-aware client or server.

---

## What is MCP (Model Context Protocol)?

MCP is a conceptual protocol that structures how LLMs consume and manage context. It includes:

- **User profile/context**: Who is the user? What are their preferences or roles?
- **Thread history**: What has been said so far in this session?
- **Tool outputs**: What tools or plugins have been invoked?
- **System guidelines**: Are there any global instructions?

A structured MCP might look like this:

```json
{
  "user": {"id": "u123", "role": "researcher"},
  "history": [
    {"role": "user", "content": "Explain reinforcement learning."},
    {"role": "assistant", "content": "Reinforcement learning is..."}
  ],
  "tools": {"retrieval": "Document ABC"},
  "instructions": "Be concise and cite references."
}
```

---

## Why Streamlit for MCP Hosting?

Streamlit allows you to:

- Rapidly prototype interactive interfaces.
- Visualize and manipulate structured data.
- Easily deploy on local servers or cloud platforms.
- Seamlessly integrate with Python backends, making it ideal for hosting an MCP client or even a mock MCP server.

---

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

## Step 1: Install Dependencies

```bash
pip install streamlit requests pydantic
```

---

## Step 2: Define the MCP Schema with Pydantic

We use Pydantic to define the data models:

```python
from pydantic import BaseModel
from typing import List, Optional, Dict

class Message(BaseModel):
    role: str  # 'user' or 'assistant'
    content: str

class MCPContext(BaseModel):
    user: Dict[str, str]
    history: List[Message]
    tools: Optional[Dict[str, str]] = {}
    instructions: Optional[str] = ""
```

---

## Step 3: Create a Streamlit MCP Client

This interface allows users to interactively send prompts and manage context.

```python
import streamlit as st
from mcp_models import MCPContext, Message
import requests

st.title("🧠 MCP Client Interface")

# Session state to keep track of history
if "history" not in st.session_state:
    st.session_state["history"] = []

user_id = st.text_input("User ID", value="u001")
user_role = st.selectbox("User Role", ["developer", "researcher", "student"])
instructions = st.text_area("System Instructions", value="Be concise and cite sources.")

prompt = st.text_input("Your message")

if st.button("Send") and prompt:
    st.session_state["history"].append(Message(role="user", content=prompt))

    # Build MCP payload
    mcp_context = MCPContext(
        user={"id": user_id, "role": user_role},
        history=st.session_state["history"],
        instructions=instructions
    )

    # Replace with your MCP Server URL
    response = requests.post("http://localhost:8000/mcp", json=mcp_context.dict())

    if response.status_code == 200:
        reply = response.json()["reply"]
        st.session_state["history"].append(Message(role="assistant", content=reply))
    else:
        st.error("Failed to contact MCP server.")

# Display the chat
for msg in st.session_state["history"]:
    st.markdown(f"**{msg.role.capitalize()}**: {msg.content}")
```

---

## Step 4: Build a Minimal MCP Server with FastAPI (Optional)

If you want to simulate the server side:

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse
from mcp_models import MCPContext

app = FastAPI()

@app.post("/mcp")
async def handle_mcp(request: Request):
    data = await request.json()
    context = MCPContext(**data)

    # For now, echo the last user message
    user_msg = context.history[-1].content if context.history else "Hello!"
    return JSONResponse({"reply": f"[Simulated Response] You said: {user_msg}"})
```

Run this server with:
```bash
uvicorn server:app --reload --port 8000
```

---

## Step 5: Running Everything

1. Start the FastAPI server (if you're using one).
2. Run the Streamlit app:

```bash
streamlit run app.py
```

You now have a functioning MCP client interface where you can send contextual messages, and the server echoes them back in a structured MCP format.

---

## Enhancements You Can Add

- 🔍 **RAG integration**: Add a vector store and retrieval function into the tools section.
- 🧠 **Memory modules**: Hook the user profile and history to a database for persistence.
- ✨ **LLM backend**: Swap out the simulated server with OpenAI, Claude, or local LLM calls.
- 🔐 **Authentication**: Add login support to personalize per user.

---

## Warp up

The Model Context Protocol (MCP) is a flexible abstraction for managing AI context clearly and modularly. Streamlit makes it easy to host a lightweight MCP client or server, giving developers an intuitive interface to experiment, iterate, and build smarter apps.
