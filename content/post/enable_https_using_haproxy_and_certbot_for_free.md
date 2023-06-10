---
title: "Enable HTTPS using HAproxy and Certbot for free"
date: 2023-05-31T04:01:48Z
draft: false
categories:
  - Technology
tags:
  - HTTPS
  - HAProxy
  - Certbot
---

# Enabling HTTPS Support for Your Website with HAProxy and Certbot: A Cost-Free Solution

## Motivation:
In today's digital landscape, securing your website with HTTPS has become more important than ever. Not only does it enhance user trust and data privacy, but it also positively impacts your website's search engine rankings. However, acquiring and managing SSL certificates can be a daunting task, often accompanied by high costs. Fortunately, there are open-source tools like HAProxy and Certbot that can help you enable HTTPS support for your website free of charge. In this blog post, we will explore how to use HAProxy and Certbot in tandem to achieve secure and encrypted communication with your website.

## Understanding HAProxy:
HAProxy is a reliable and high-performance load balancer and proxy server. It is renowned for its flexibility and scalability, making it an ideal choice for managing traffic between clients and backend servers. In the context of enabling HTTPS, HAProxy acts as a reverse proxy, terminating SSL connections and forwarding requests to your web server.

## Obtaining SSL Certificates with Certbot:
Certbot, developed by the Electronic Frontier Foundation (EFF), is a free and automated tool for obtaining and renewing SSL certificates. It simplifies the process of setting up HTTPS by generating and configuring SSL certificates for your domain. Certbot supports multiple verification methods, including HTTP-based challenges and DNS-based challenges, allowing you to choose the most suitable option for your environment.

## Step-by-Step Instructions:
Here's a step-by-step guide to setting up HTTPS support for your website using HAProxy and Certbot. Please note that the commands I have provided are for Ubuntu OS. You may need to modify these based on your OS:

- 1: Install HAProxy and Certbot:
Begin by installing HAProxy and Certbot on your server. Depending on your operating system, you can use package managers like apt, yum, or dnf to install them.

```
sudo apt-get install haproxy
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

- 2: Configure HAProxy:
Next, configure HAProxy to handle SSL termination. Update the HAProxy configuration file (typically located at /etc/haproxy/haproxy.cfg) to include the necessary SSL-related directives. Ensure that you define frontend and backend sections, specify the SSL certificate paths, and configure the appropriate SSL settings.

```
sudo service haproxy stop
```

Edit HAProxy config with your editor of choice. In my case I use the mighty VIM editor:

```
sudo vim /etc/haproxy/haproxy.cfg
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

- You can find a sample config file [here](https://skarlso.github.io/2017/02/15/how-to-https-with-hugo-letsencrypt-haproxy/#haproxy-configuration).
Make sure to add your pem file under frontend www-https and update the IP under backend www-backend with your server IP.
- You can prepare your pem file by following the instructions [here](https://skarlso.github.io/2017/02/15/how-to-https-with-hugo-letsencrypt-haproxy/#haproxy-1).

Now start HAProxy service

```
sudo service haproxy start
```

- 3: Automate Certificate Renewal:
SSL certificates have a limited validity period, usually 90 days. To ensure uninterrupted HTTPS support, set up an automated renewal process for your certificates using Certbot's built-in renewal mechanism or by scheduling a cron job.


```
sudo certbot certonly --standalone
sudo certbot renew --dry-run
```


- 4: Test and Monitor:
Verify that your HTTPS setup is functioning correctly by accessing your website over a secure connection. Additionally, monitor the certificate expiration dates and renewals to avoid any potential disruptions.

If you face any problem, you can always check HAProxy log at /var/log/haproxy.log to get more insights about potential issues.

## [Optional] Additional Considerations:
While HAProxy and Certbot provide a cost-effective solution for enabling HTTPS support, there are a few additional aspects to consider:
a. HSTS (HTTP Strict Transport Security): Implementing HSTS headers ensures that clients always communicate with your website over HTTPS, even if they initially access it via an insecure connection.

b. OCSP Stapling: Enable OCSP (Online Certificate Status Protocol) stapling to enhance the speed and security of certificate validation by caching the certificate's revocation status.

c. Content Security Policies (CSP): Implementing CSP adds an extra layer of security by defining which sources the browser can load content from, mitigating the risk of malicious code injection.
