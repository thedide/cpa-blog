---
title: "Host Multiple Domains Served by haproxy on One EC2 Server"
date: 2023-06-28T19:44:36-07:00
categories:
  - Technology
tags:
  - EC2
  - AWS
  - httpd
  - Virtual Host
  - Ubuntu
  - haproxy
  - Wordpress
  - Hugo
---

In the [previous](https://www.comparepriceacross.com/post/host_multiple_domains_on_one_ec2_server_using_httpd/) post, we explained how to serve multiple domains on one server using apache http server virtual host. In this post we explain the same concept except we will be using haproxy for load balancing and https support.
How each domain is served really does not matter here but in my case I'm hosting two websites one served by hugo on 8080 port and another served by wordpress on port 8082.


## Prerequisites

- Knowing what it means to host a website
- Basic knowledge about Apache http server or any other http server
- Basic understanding of load balancing
 
## Step by step instructions

- Run your hugo server on your desired port. In my case I'm running it on port 8080:
```
./hugo server --bind=0.0.0.0 --port=8080 --baseURL=<your_domain_name_goes_here> --appendPort=false
```

- Configure wordpress to run on a custom port. In my cse I'm running it on 8082. I do this by touching the apache config file located at:
```
/etc/apache2/ports.conf
```
and adding 'Listen 8082" to the file (if you already have a 'listen' statement there, just replace the default port with your desired port number).

- Edit the content of the default conf located at:
```
/etc/apache2/sites-available/000-default.conf
```
and add the right port number to the 'VirtualHost' tag. In my case it'll look like '<VirtualHost *:8082>'
Re-start apache server by running 'sudo systemctl restart apache2'.

- Edit haproxy file located at:
```
/etc/haproxy/haproxy.cfg
```
and under 'frontend www-http' add the following lines:
```
acl ACL_your_domain_name.com hdr(host) -i your_domain_name.com www.your_domain_name.com
use_backend be_your_domain_name.com if ACL_your_domain_name.com
```
Please note that your domain does not need to end in '.com' and it can have any arbitrary extension.

Add a new section in your haproxy.cfg file as follows:
```
backend be_your_domain.com
  server your_domain_name    127.0.0.1:8082 check
```
re-start haproxy by running 'sudo service haproxy restart' and you should be done.

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

## HTTPS support

You can use [letsencrypt](https://letsencrypt.org/) to get ssl certificates for your website free of charge. I have a [blog post](https://www.comparepriceacross.com/post/enable_https_using_haproxy_and_certbot_for_free/) on this topic, but as a refresher:

- Run the following commands to prepare a certificate for your website:
```
 sudo ls /etc/letsencrypt/live/
 # run this to create certificate for raw and www versions of your domain name. Make sure to replace <your_domain_name> with your_domain_name :)
 sudo certbot certonly --standalone -d your_domain_name.com -d www.your_domain_name.com
 # run this to automatically re-new your certificate
 1946  sudo certbot renew --dry-run
 sudo -E bash -c 'cat /etc/letsencrypt/live/your_domain_name.com/fullchain.pem /etc/letsencrypt/live/your_domain_name.com/privkey.pem > /etc/haproxy/certs/your_domain_name.com.pem'
```

- Edit your '/etc/haproxy/haproxy.cfg' file and add the following content:
```
frontend www-https
    # Bind 443 with the generated letsencrypt cert.
    bind *:443 ssl crt /etc/haproxy/certs/your_domain_name.com.pem

    acl ACL_your_domain_name.com hdr(host) -i your_domain_name.com www.your_domain_name.com
    use_backend be_your_domain_name.com if ACL_your_domain_name.com

# You should have already added this based on the step by step instructions above
backend be_your_domain.com
  # This is the new command, redirecting http traffic to https
  redirect scheme https code 301 if !{ ssl_fc }
  # You should have already added this based on the step by step instructions above
  server your_domain_name    127.0.0.1:8082 check    
```

- go to your wordpress admin panel (you can do this by directly updating the wp_options table under wordpress data base or even editing your wordpress php files) and make sure that "home" and "site_url" variable use https rather than http, otherwise your blog or admin panel pages style may not work properly.

- Re start your haproxy