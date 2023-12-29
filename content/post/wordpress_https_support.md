---
title: "Wordpress https support"
date: 2023-12-29T08:27:39-08:00
categories:
  - Technology
tags:
  - Wordpress
  - AWS
  - https
---

Securing your website with HTTPS is crucial for protecting user data, boosting SEO, and building trust. If you're using WordPress on AWS, here's a guide to smoothly enable HTTPS support:

1. Update WordPress Site URL:

Access your WordPress admin dashboard.
Navigate to Settings > General.
In the WordPress Address (URL) and Site Address (URL) fields, add https://www to the beginning of your URLs.
Click Save Changes.

2. Create AWS Entry for HTTP to HTTPS Redirection:

Log in to your AWS Management Console.
Access the Route 53 or CloudFront service (depending on your setup).
Create a new rule or configuration to redirect HTTP traffic to the HTTPS (www) version of your website.
Ensure this rule applies to all HTTP requests.

3. Modify wp-config.php File:

Access your WordPress site's files using an FTP client or file manager.
Locate the wp-config.php file in the root directory (typically /srv/www/<your_website_folder>/).
Open the file in a text editor and add the following lines at the end:

```
define('FORCE_SSL_ADMIN', true);

if (strpos($_SERVER['HTTP_X_FORWARDED_PROTO'], 'https') !== false) {
    $_SERVER['HTTPS'] = 'on';
} else {
    $_SERVER['HTTPS'] = 'off';
}
```

Save the changes to wp-config.php.

4. Clear Cache and Test:

Clear your browser cache and WordPress cache (if applicable).
Visit your website using both http:// and https:// URLs. It should automatically redirect to the HTTPS version.
Check your browser's security indicators to confirm the HTTPS connection is valid.
Additional Tips:

Obtain an SSL/TLS Certificate: You'll need an SSL/TLS certificate from a trusted certificate authority (CA) to enable HTTPS. AWS Certificate Manager (ACM) offers free certificates for use with AWS services. read more [here](https://www.comparepriceacross.com/post/host_multiple_domains_on_one_ec2_server_using_haproxy/) about how you can use letsencrypt to get SSL/TLS certificates and use for your website.
Test Thoroughly: Ensure all website pages and functionalities work correctly over HTTPS.
Monitor Certificate Expiration: Set reminders to renew your SSL/TLS certificate before it expires.
Congratulations! Your WordPress website is now securely served over HTTPS!