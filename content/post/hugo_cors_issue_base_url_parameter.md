---
title: "Hugo CORS Issue and the baseUrl Parameter"
date: 2023-04-22T22:24:13Z
categories:
  - Technology
tags:
  - CORS
  - Hugo
  - baseUrl
---

# CORS issue hosting a Hugo website
Hugo is a popular static site generator that simplifies the process of creating and deploying websites. One of the key features of Hugo is its ability to configure the base URL for your website. However, if the base URL is set to the www version of the URL, you may receive a cross-origin reference error when attempting to access the naked version of the URL without the www prefix.

This error occurs because the www version of the URL and the naked version of the URL are considered different origins by web browsers. In other words, the naked version of the URL is not allowed to access resources on the www version of the URL due to cross-origin resource sharing (CORS) restrictions.

# How to solve the issue?
To solve this issue, you can add an A record in AWS Route 53 to redirect the naked version of the URL to the www version of the URL. Here are the steps to follow:

## AWS Route 53

1- Log in to your AWS account and navigate to the Route 53 console.

2- Click on the name of the hosted zone for your domain.

3- Click the "Create Record Set" button.

4- In the "Name" field, enter the naked version of your domain name (e.g. example.com).

5- In the "Type" dropdown, select "A - IPv4 address".

6- In the "Value" field, enter the IP address of an S3 bucket that you will create for URL redirecting.

7- Click the "Create" button.

Now you need to set up the S3 bucket and redirect the naked version of the URL to the www version of the URL. Here are the steps to follow:

## AWS S3

1- Create an S3 bucket with the same name as the naked version of your domain name (e.g. example.com).

2- Select the newly created bucket, and click the "Properties" tab.

3- Click on the "Static website hosting" card.

4- Select "Redirect requests".

5- In the "Target bucket or domain" field, enter the www version of your domain name (e.g. www.example.com).
6- Click the "Save changes" button.

With these steps, you have set up the A record and URL redirecting for your domain. Now, when someone enters the naked version of your domain name in their browser (e.g. example.com), they will be automatically redirected to the www version of your domain (e.g. www.example.com). This ensures that your website resources can be accessed from both versions of the URL without any cross-origin reference errors.

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

## Restart your Hugo server
Additionally, it's important to update the base URL in your Hugo configuration to match the www version of your domain name. This will ensure that all internal links on your website point to the correct URL and prevent any potential SEO issues caused by duplicate content.

To update the base URL in your Hugo configuration, follow these steps:

1- Open your Hugo site's config.toml file.

2- Locate the line that specifies the base URL and change it to the www version of your domain name (e.g. baseurl = "https://www.example.com").

3- Save the changes to your config.toml file.

4- Now, when you generate your Hugo site and deploy it to S3, all internal links will point to the www version of your domain name. Any visitors to your site who enter the naked version of your domain name will be redirected to the www version, ensuring that all resources are accessible without any cross-origin reference errors.

In conclusion, setting the base URL in Hugo to the www version of your domain name can cause cross-origin reference errors when attempting to access the naked version of your domain name. By adding an A record in AWS Route 53 and redirecting the naked version of your domain name to the www version using an S3 bucket, you can avoid these errors and ensure that all resources on your site are accessible from both versions of your domain name. Don't forget to update your Hugo configuration to reflect the changes to your base URL and ensure that all internal links point to the correct URL.
