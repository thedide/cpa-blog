---
title: "Too Many Redirects Error After Enabling HTTPS for Wordpress"
date: 2023-06-29T23:37:04-07:0Z
draft: true
categories:
  - Technology
tags:
  - Wordpress
  - https
  - too many redirects
---


Enabling HTTPS (Hypertext Transfer Protocol Secure) for your WordPress website is crucial for enhancing security and establishing trust with your visitors. However, sometimes after enabling HTTPS, you may encounter the dreaded "Too Many Redirects" error. Don't worry; in this post, we'll guide you through the steps to resolve this issue and ensure a smooth transition to HTTPS.

Initially after enabling https I was not able to log in to the wordpress admin panel and I was getting too many redirect error. I solved the issue by pasting the following block of code in my wp-config.php file:

```
define('FORCE_SSL_ADMIN', true);

if( strpos($_SERVER['HTTP_X_FORWARDED_PROTO'], 'https') !== false )
   $_SERVER['HTTPS'] = 'on';
else
   $_SERVER['HTTPS'] = 'off';
```

However, after using the code above, I faced a new issue. I was getting "Sorry, You Are Not Allowed to Access This Page" after logging in.
After doing some research I learned that I needed to put the above snippet immediately after the opening "php" tag at the beginning of my wp-config.php file rather than the bottom of the file where I had put it :)