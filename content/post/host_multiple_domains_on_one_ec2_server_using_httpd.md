---
title: "Host Multiple Domains Served by Apache Http Server on One EC2 Server"
date: 2023-06-27T18:01:14-07:00
draft: true
categories:
  - Technology
tags:
  - EC2
  - AWS
  - httpd
  - Virtual Host
  - RHEL
---

## Motivation
Hosting multiple domains on the same EC2 server using httpd and virtual host can provide several benefits for website owners or administrators. Here are a few reasons why one would consider implementing this setup:

1. **Cost Efficiency**: By hosting multiple domains on a single EC2 server, you can optimize resource utilization and reduce costs. Instead of provisioning separate servers for each domain, you can consolidate them onto a single server, resulting in significant savings in terms of infrastructure expenses.

2. **Simplified Management**: Managing multiple domains from a single server simplifies administrative tasks. You can centralize configurations, updates, and maintenance, making it easier to ensure consistency and streamline management processes. It also reduces the need for monitoring and maintaining multiple servers.

3. **Flexibility and Customization**: By configuring virtual hosts with httpd, you can tailor the server environment for each domain individually. This allows you to set specific settings, apply different security measures, and customize the server behavior according to the requirements of each website.

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

## step-by-step guide on how to host multiple domains on the same EC2 server with RHEL OS, using httpd and virtual host

1- Install httpd:

```
sudo yum install httpd
```

2- Create the necessary directories:
```
sudo mkdir -p /etc/httpd/sites-available
sudo mkdir -p /etc/httpd/sites-enabled
```

3- Edit the httpd.conf file:
```
sudo vi /etc/httpd/conf/httpd.conf
```

Add the following line to the file:

```
IncludeOptional sites-enabled/*.conf
```

4- Create the directories for each domain and upload website contents:
```
sudo mkdir -p /var/www/first_domain
sudo mkdir -p /var/www/second_domain
```

Upload your website contents to the corresponding directories.

5- Create a configuration file for each domain:
```
sudo vi /etc/httpd/sites-available/domain_name.conf
```

Replace "domain_name" with the actual domain name (e.g., test.com) and add the following content:

```
<VirtualHost *:80>
    DocumentRoot "/var/www/domain_name/"
    ServerName www.domain_name
    ServerAlias domain_name
    CustomLog /var/log/httpd/domain_name_access.log combined
    ErrorLog /var/log/httpd/domain_name_error.log
</VirtualHost>
```

6- Enable the virtual host for each domain:
```
sudo ln -s /etc/httpd/sites-available/domain_name.conf /etc/httpd/sites-enabled/domain_name.conf
```

7- Restart the httpd service:
```
sudo systemctl restart httpd
```