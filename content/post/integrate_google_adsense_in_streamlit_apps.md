---
title: "Integrate Google AdSense in Streamlit Apps"
date: 2024-02-16T07:19:51-08:00
categories:
  - Technology
tags:
  - Streamlit
  - Adsense
  - Google
  - Python
  - Web development
  - Web app
  - Advertising
  - Monetization
  - Beautiful Soup
---

## Monetize Your Streamlit App: Integrating Google AdSense for Seamless Advertising

So you've built a fantastic website with Streamlit, the user-friendly framework that makes creating web apps with Python a breeze. You're sharing insightful content and providing valuable tools, but wouldn't it be nice to earn a little something back for your efforts? That's where Google AdSense comes in!

**Streamlit: Your One-Stop Shop for Web Apps**

If you're new to the world of Streamlit, let's rewind a bit. Streamlit simplifies web development by allowing you to code your app's logic purely in Python. No more struggling with complex web frameworks or juggling front-end and back-end technologies. Just write Python, and Streamlit takes care of the rest, presenting your app in a clean, responsive interface.

**What is Google AdSense and Why Use It?**

Now, onto the star of the show: Google AdSense. Essentially, AdSense is an advertising platform that connects publishers (like you, with your Streamlit app) with advertisers looking to reach relevant audiences. You integrate a small snippet of code into your website, and Google takes care of finding and displaying targeted ads. You earn money whenever someone clicks on an ad, making it a passive income stream for your efforts.

**Why Advertise on Your Streamlit App?**

Several reasons make adding AdSense to your Streamlit app an attractive option:

* **Monetization:** Turn your web app into a revenue generator, rewarding your time and effort.
* **Targeted Ads:** Google ensures ads displayed are relevant to your content, enhancing user experience instead of disrupting it.

**Adding AdSense to Your Streamlit App: A Step-by-Step Guide**

Remember, while Streamlit doesn't officially endorse injecting arbitrary Javascript, ethical and responsible use of AdSense adheres to their user policies. Here's a general approach, but always double-check for updates and specific limitations:

### **Get Your AdSense Code:** 
Create an AdSense account and grab the code snippet they provide.

### **Prepare Your Streamlit App:** 
Choose where you want to display the ads (e.g., sidebar, footer).

### **Inject the Code:** 
This step requires adding Google Adsense script to your page.

Adding google Adsense script to your Streamlit page is not straightforward. I recommend directly manipulating your streamlit index page by utilizing BeautifulSoup and adding required checkings in place to avoid re-injecting the adsense script if it already exists. This approach is based on the discussions [here](https://discuss.streamlit.io/t/google-analytics-and-streamlit/29385/2):

Let's learn more about Beautiful Soup first:
Beautiful Soup is a popular Python library designed to "parse" HTML and XML documents. Think of it as a tool that takes messy, tangled code and neatly extracts the important bits. Instead of manually navigating complex tags and attributes, Beautiful Soup lets you access specific elements like paragraphs, headings, or links easily. But it's not just about reading HTML, you can also use Beautiful Soup to **write** new HTML content! By feeding it structured data and defining the desired structure, you can generate HTML elements tailored to your needs. Imagine creating lists of products with descriptions, formatting blog posts with images, or dynamically building tables, all through code that's simple and readable. In essence, Beautiful Soup empowers you to both understand and manipulate HTML effectively, making it a versatile tool for web scraping, data extraction, and even generating your own web content.


```
    adsense_url = "https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"
    GA_AdSense = """
      <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
      <ins class="adsbygoogle"
          style=<your_ad_style>
          data-ad-client=<your_client_id>
          data-ad-slot=<your_ad_slot>
          data-ad-format=<your_ad_format>
          data-full-width-responsive="true"></ins>
      <script>
          (adsbygoogle = window.adsbygoogle || []).push({});
      </script>
    """

    # Insert the script in the head tag of the static template inside your virtual
    index_path = pathlib.Path(st.__file__).parent / "static" / "index.html"
    logging.info(f'editing {index_path}')
    soup = BeautifulSoup(index_path.read_text(), features="html.parser")
    if not soup.find(script, src=adsense_url): 
        bck_index = index_path.with_suffix('.bck')
        if bck_index.exists():
            shutil.copy(bck_index, index_path)  
        else:
            shutil.copy(index_path, bck_index)  
        html = str(soup)
        new_html = html.replace('<head>', '<head>\n' + GA_AdSense)
        index_path.write_text(new_html)
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

This method works by directly modifying the head tag of the static template index.html in Streamlit. Wrap this code in a method and invoke the method on your page load. Here's the detailed breakdown:

**1. Setting up AdSense information:**

* `adsense_url`: Defines the URL to load the AdSense Javascript library.
* `GA_AdSense`: This string contains the HTML code for the AdSense script, including placeholders for your specific `client_id`, `ad_slot`, and `ad_format`.

**2. Finding the target location:**

* `index_path`: Identifies the static HTML template file (`index.html`) within the Streamlit application directory.
* `soup`: This variable uses BeautifulSoup to parse the HTML content of the template file.

**3. Checking for existing AdSense code:**

* `soup.find(script, src=adsense_url)`: Checks if the AdSense script already exists in the `<head>` tag of the HTML.

**4. Handling backup and insertion:**

* If the script isn't found:
    * `bck_index`: Defines a backup filename for the original template.
    * `shutil.copy`: Creates a backup copy of the original template (if not already done).
    * The HTML content is read and transformed:
        * `html = str(soup)`: Converts the parsed HTML object to a string.
        * `new_html = html.replace('<head>', '<head>\n' + GA_AdSense)`: Inserts the AdSense script immediately after the opening `<head>` tag.
    * Finally, the modified HTML is written back to the original template file.

**Overall, this code automates the process of adding Google AdSense advertising to a Streamlit application's static template, ensuring a backup exists before making any changes.**

**Important notes:**

* Remember to replace the placeholders in `GA_AdSense` with your actual AdSense credentials.
* Injecting arbitrary Javascript into web pages can have security and ethical implications. Ensure you understand and follow the AdSense policies and user agreement when using this code.


### **Test and Monitor:** 
Ensure the ads display correctly and track your earnings through the AdSense dashboard.

**Important Note:** While this overview provides a general understanding, always refer to Streamlit's documentation and community resources for the latest guidelines and recommended practices.

**Ready to Earn?**

By integrating Google AdSense, you can turn your Streamlit app into a revenue-generating platform. Just remember to prioritize user experience and adhere to ethical advertising practices. So, dive into the world of AdSense and watch your Streamlit app blossom into a rewarding project!
