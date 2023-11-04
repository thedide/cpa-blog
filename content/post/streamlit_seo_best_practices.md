---
title: "Streamlit SEO Best Practices"
date: 2023-11-04T07:06:52-07:00
draft: false
categories:
  - Technology
tags:
  - Streamlit
  - SEO
  - Search Engine Optimization
  - Google Analytics
---

Read our previous [post](https://www.comparepriceacross.com/post/deploy_streamlit_app_on_sub_domain_with_haproxy_and_aws/) to learn how to deploy your Streamlit website in your custom sub domain free of charge (assuming you already own a live domain).
Streamlit is a popular Python framework for building and deploying web applications. It makes it easy to create interactive UIs with data visualization and machine learning capabilities.

Although Streamlit makes website building easy, you need to know how to set your page metadata to make your pages more discoverable by search engines. In this post we share best practices for make your Streamlit apps SEO friendly.

## Page name

First thing to consider is to use a descriptive page name. Pick a name for your page/app that is self explanatory so that everyone (including search engines) can get a high level idea about your content by reading the page name.
Avoid using generic titles like "My Streamlit App" or "Data App."

## Page meta data

To set your page meta data including your page title and icon you can use set_page_config method of Streamlit.

```
import streamlit as st

st.set_page_config(
    page_title="Your page title here",
    page_icon="Your fav icon here",
    layout="wide", #choose any layout you want
    initial_sidebar_state="expanded" #set initial side bar state
)
```
According to [Streamlit official documentation](https://docs.streamlit.io/streamlit-community-cloud/share-your-app/indexability), search engines favor content set using st.header and st.text. Use st.text on top of your page to set a clear description for your app/page.

Looks like there is a new component for Streamlit to set tags for your page as well. I have personally not used this feature yet but based on my research you can use the following code snippet.

```
pip install streamlit-tags

import streamlit as st
from streamlit_tags import st_tags

keywords = st_tags(‘Enter Keyword:’, ‘Press enter to add more’, [‘One’, ‘Two’, ‘Three’])

st.write(keywords)
```

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

## Google Analytics
Adding google analytics script to your Streamlit page is not straightforward. There are posts out there suggesting to use st.markdown by setting unsafe_allow_html=True which does not seem to work at least for me. Instead I recommend the following code snippet based on the discussions [here](https://discuss.streamlit.io/t/google-analytics-and-streamlit/29385/2):

```
    GA_JS = """
    <!-- Global site tag (gtag.js) - Google Analytics -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-**********"></script>
    <script>
        window.dataLayer = window.dataLayer || [];
        function gtag(){dataLayer.push(arguments);}
        gtag('js', new Date());

        gtag('config', 'G-**********');
    </script>
    """

    # Insert the script in the head tag of the static template inside your virtual
    index_path = pathlib.Path(st.__file__).parent / "static" / "index.html"
    logging.info(f'editing {index_path}')
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


inject_ga()
```

This method works by directly modifying the head tag of the static template index.html in Streamlit.