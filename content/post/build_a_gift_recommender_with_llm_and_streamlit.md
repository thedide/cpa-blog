---
title: "Build a Gift Recommender with LLM and Streamlit"
date: 2024-01-12T21:32:49-08:00
featured_image: "../../gift_recommender.png"
categories:
  - Technology
tags:
  - Gift recommender
  - LLM
  - Large Language Models
  - Streamlit
  - Chat GPT
  - gpt-3.5-turbo
---

## Unwrap Inspiration: Building a Gift Recommender with LLMs and Streamlit

Struggling to find the perfect gift? Drowning in a sea of online options? Worry not, friend! Let's dive into the exciting world of building a gift recommender app using the power of Large Language Models (LLMs) and Streamlit, a Python library for creating beautiful and interactive data apps.

**The Spark of an Idea:**

Imagine a user-friendly web app where you choose a few simple details – recipient's age, gender, occasion, and budget – and voila! An LLM conjures up a list of personalized gift ideas. No more frantic Google searches or hours spent in crowded malls. Sounds magical, right?

**Our Blueprint:**

This post is based on our gift recommender solution ([https://gift.comparepriceacross.com/](https://gift.comparepriceacross.com/)). Here's how you can replicate its key features:

<p align="center">
<img style="border: 3px solid #e9f1f2" src="../../gift_recommender.png" width="800" alt="Gift recommender user interface" />
</p>

**1. User Input:**

* **Gender:** Radio buttons for "male," "female," or "unisex."
* **Age:** A slider to select the recipient's age.
* **Occasion:** A dropdown menu with various celebratory moments like birthdays, holidays, or graduations.
* **Price Range:** A slider to set the desired budget.

**2. LLM Power:**

* **OpenAI's GPT-3:** This powerful LLM will take the user's input and generate a list of gift ideas in natural language.

**3. Streamlit Magic:**

* **Interactive Interface:** Streamlit lets us build the app's UI with sliders, buttons, and text boxes, making it user-friendly and visually appealing.
* **Real-time Recommendations:** As the user selects options, the LLM generates gift ideas displayed instantly, creating a dynamic and engaging experience.

**Building the Dream:**

Let's break down the code into bite-sized pieces:

1. **Import Libraries:** We'll need Streamlit, OpenAI, and other modules for handling UI and API calls.

```python
import streamlit as st
import streamlit.components.v1 as components
import openai
import time
```

2. **User Input:** Use Streamlit widgets like radio buttons, sliders, and dropdowns to gather information from the user.

```python
gender = st.radio("gender", ["👨🏻‍🦰", "👱🏻‍♀️", "😶"], captions = ["male", "female", "unisex"])
age = st.slider("age", min_value=0, max_value=80, step=1)
price_range = st.slider("price (USD)", 0, 1000, (25, 100))
# define occasion as well
```

3. **Prepare Prompt:** Construct a clear and concise prompt for the LLM, including the user's chosen options and desired number of gift ideas.

4. **LLM Call:** Use OpenAI's API to send the prompt to the GPT-3 model and receive the generated list of recommendations.

```python
model_name="gpt-3.5-turbo"
with st.spinner('Generating gift ideas...'):
    completion = openai.ChatCompletion.create( # Change the function Completion to ChatCompletion
    prompt = "Set the prompt variable to give instructions to LLM how you want to generate gift ideas based on user's input."
    model = 'gpt-3.5-turbo',
    messages = [ # Change the prompt parameter to the messages parameter
        {'role': 'user', 'content': prompt}
    ],
    temperature = 0
    )
    generated_query = completion['choices'][0]['message']['content']
```

5. **Display Results:** Format the LLM's output into a visually appealing list or table within the Streamlit app.

```python
st.markdown(generated_query)
```

**Wrapping Up:**

With this framework, you can build a personalized gift recommender app that takes the guesswork out of gifting. Imagine the joy of seeing recipients' faces light up with your thoughtfully chosen presents, all thanks to the power of AI!

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

## Beyond the Basics:

 ### Unwrapping Insights with Google Analytics:

As your gift recommender app takes flight, it's essential to understand how users interact with it. Google Analytics, a powerful tool for web analytics, can provide valuable insights about user behavior, preferences, and engagement patterns.

**Integrating Google Analytics with Streamlit:**

**1. Obtain Your Tracking ID:**
   - Create a free Google Analytics account and set up a property for your app.
   - You'll receive a unique Tracking ID that identifies your app's data within Google Analytics.

**2. Inject the Tracking Code:**
   - Here's the code snippet to add Google Analytics support to your Streamlit app:

```python
import pathlib
from bs4 import BeautifulSoup
import logging
import shutil
def inject_ga():
    GA_ID = "google_analytics"  # Replace with your actual Tracking ID

    GA_JS = """
    <script async src="https://www.googletagmanager.com/gtag/js?id=your_tracking_id"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', 'your_tracking_id');  # Replace with your Tracking ID
    </script>
    """

    # Insert the script into the index.html file
    index_path = pathlib.Path(st.__file__).parent / "static" / "index.html"
    soup = BeautifulSoup(index_path.read_text(), features="html.parser")
    if not soup.find(id=GA_ID):
        bck_index = index_path.with_suffix('.bck')
        if bck_index.exists():
            shutil.copy(bck_index, index_path)  
        else:
            shutil.copy(index_path, bck_index)  
        html = str(soup)
        new_html = html.replace('<head>', '<head>\n' + GA_JS)
        index_path.write_text(new_html)
```
 **Here's a breakdown of what the code block does:**

**2.1. Checks for Existing GA Code:**

- `if not soup.find(id=GA_ID):`: This line checks if the Google Analytics tracking code (identified by the ID `GA_ID`) is already present within the `index.html` file.

**2.2. Creates a Backup (if Needed):**

- `bck_index = index_path.with_suffix('.bck')`: It creates a backup file named `index.html.bck` to ensure that any existing content is preserved before modifications.
- `if bck_index.exists(): ... else: ...`: It checks if the backup file already exists. If it does, it copies the backup to the original `index.html`. If not, it creates a new backup of the current `index.html`.

**2.3. Injects the GA Code:**

- `html = str(soup)`: It converts the BeautifulSoup object (`soup`, representing the parsed HTML content) into a string for manipulation.
- `new_html = html.replace('<head>', '<head>\n' + GA_JS)`: It inserts the Google Analytics JavaScript code (`GA_JS`) into the `<head>` section of the HTML, ensuring that it loads early in the page.
- `index_path.write_text(new_html)`: It saves the modified HTML content back to the `index.html` file, effectively adding the GA tracking code.

**Key Points:**

- The code only injects the GA code if it's not already present, preventing duplication.
- It creates a backup file for safety, allowing you to revert changes if needed.
- It modifies the HTML directly to ensure proper tracking functionality.


**3. Call the Function:**
   - Add `inject_ga()` to your app's code to initiate tracking.

**Unlocking Valuable Insights:**

With Google Analytics integrated, you can now explore a treasure trove of data:

- **User Demographics:** Understand your audience's age, gender, location, and interests.
- **Traffic Sources:** See how users find your app (organic search, social media, etc.).
- **Popular Features:** Identify the most used gift recommendation options.
- **User Flow:** Track how users navigate through your app, pinpointing areas for improvement.
- **Engagement Metrics:** Measure time spent on the app, pages visited, and conversion rates.

Use these insights to refine your app's design, content, and features, ultimately enhancing the user experience and making your gift recommender even more delightful!


### Personalization 
Refine the LLM prompt to incorporate the recipient's hobbies or interests for even more targeted suggestions.

### Visual Flair 
Enhance the app with images or links to purchase recommended items.

### Community Connection:

Allow users to share their chosen gifts and create a curated gift database.

Building a gift recommender app is not just about coding; it's about creativity and understanding the user's experience. With LLMs and Streamlit, you can unlock a world of possibilities and help others spread the joy of gift-giving. So, get coding, embrace the magic of AI, and let's make gifting truly personal and delightful!

Remember, this is just a starting point. Feel free to customize the app, explore different LLMs, and add your own unique features to make it truly your own. Happy coding and happy gifting!
