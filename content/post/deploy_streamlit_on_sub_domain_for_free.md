---
title: "Deploy streamlit app on sub domain for free"
date: 2023-11-03T22:15:20-07:00
draft: false
categories:
  - Technology
tags:
  - Streamlit app
  - haproxy
  - subdomain
  - aws
  - route 53
---

Streamlit is a popular Python framework for building and deploying web applications. It makes it easy to create interactive UIs with data visualization and machine learning capabilities.

If you have implemented an application/website with streamlit you might be wondering how I can deploy my app online so that
others can access and use it. Streamlit provides neat and easy functionality to deploy your app to a default domain. However, if you want to deploy your app to your own custom domain you may need some additional work. This post helps you   deploy your streamlit to your own custom sub domain free of charge (assuming you already own a domain).

In this post we assume:

- You own a domain (e.g. example.com)
- You want to deploy your streamlit app to a custom subdomain on your own domain (e.g. test.example.com)
- You have an EC2 instance and use Route 53 for routing your traffic
- You use haproxy for loadbalancing
- You use certbot to create ssl certificate

Follow these stesp to accomplish your goals:

## Create ssl certificate

```
sudo certbot certonly --standalone -d test.example.com -d www.test.example.com
sudo -E bash -c 'cat /etc/letsencrypt/live/test.example.com/fullchain.pem /etc/letsencrypt/live/test.example.com/privkey.pem > /etc/haproxy/certs/test.example.com.pem'
```

## Create alias records in Route 53

- Go to Route 53 in aws console.
- Go to hosted zones and select your domain.
- create a record of type A with name test.example.com, and add your server ip address as the value.
- create a record of type A with name www.test.example.com, set the alias flag as true and point your record to the entry you created in the previous step.

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

## Modify haproxy config file

You can get an idea of what changes you need to make to your haproxy config file by looking at the following snippet.

```
frontend www-http
    # Redirect HTTP to HTTPS
    bind *:80
    # Adds http header to end of end of the HTTP request
        acl ACL_test.example.com hdr(host) -i test.example.com www.test.example.com
        use_backend be_test if ACL_test.example.com

    reqadd X-Forwarded-Proto:\ http
        #use_backend special if { dst_port 8083 }
    # Sets the default backend to use which is defined below with name 'www-backend'
    default_backend www-backend

# If the call is HTTPS we set a challenge to letsencrypt backend which
# verifies our certificate and than direct traffic to the backend server
# which is the running hugo site that is served under https if the challenge succeeds.
frontend www-https
    # Bind 443 with the generated letsencrypt cert.
    bind *:443 ssl crt /etc/haproxy/certs/test.example.com.pem crt /etc/haproxy/certs/test.example.com.pem

        acl ACL_text.example.com hdr(host) -i test.example.com www.test.example.com
        use_backend be_test if ACL_test.example.com

    # set x-forward to https
    reqadd X-Forwarded-Proto:\ https
    # set X-SSL in case of ssl_fc <- explained below
    http-request set-header X-SSL %[ssl_fc]
    # Select a Challenge
    acl letsencrypt-acl path_beg /.well-known/acme-challenge/
    # Use the challenge backend if the challenge is set
    use_backend letsencrypt-backend if letsencrypt-acl
    default_backend www-backend

backend www-backend
   # Redirect with code 301 so the browser understands it is a redirect. If it's not SSL_FC.
   # ssl_fc: Returns true when the front connection was made via an SSL/TLS transport
   # layer and is locally deciphered. This means it has matched a socket declared
   # with a "bind" line having the "ssl" option.
   redirect scheme https code 301 if !{ ssl_fc }
   # Server for the running hugo site.
   server www-1 <YOUR_SERVER_IP_ADDRESS_HERE>:8080 check

backend be_test
        redirect scheme https code 301 if !{ ssl_fc }
        server test <YOUR_SERVER_IP_ADDRESS_HERE>:8501 check 
```

## Restart haproxy
```
sudo service haproxy restart
```

## Open corresponding port on your EC2 instance

- Go to aws console and navigate to your EC2 instance.
- Go to the security tab and click on the security group.
- Edit inbound rules and add two rules (the rules the way I've defined them here let folks from any ip address access your Streamlit app, you may want to add more restrictions if needed for your use case).
- 1. Custom TCP on port 8501 (default Streamlit port, you need to change this if you are using a non-default port) and insert 0.0.0.0/0 
- 2. Custom TCP on port 8501 (default Streamlit port, you need to change this if you are using a non-default port) and insert ::/0

## Hide Streamlit style

In case you want to hide Streamlit default footer use the following code snippet:

```
hide_streamlit_style = """
            <style>
            #MainMenu {visibility: hidden;}
            footer {visibility: hidden;}
            </style>
            """
st.markdown(hide_streamlit_style, unsafe_allow_html=True)
```


## Check your new sub domain served by Streamlit

You are done. Go to your browser and check out your new sub domain (in this case test.example.com).
If you are interested in learning best SEO practices for web pages built with Streamlit read our blog post [here](https://www.comparepriceacross.com/post/streamlit_seo_best_practices/).