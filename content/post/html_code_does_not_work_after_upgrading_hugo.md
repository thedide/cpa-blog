---
title: "HTML code does not work after upgrading Hugo"
date: 2023-05-30T04:10:50Z
draft: true
categories:
  - Technology
tags:
  - Hugo
  - HTML
  - markup goldmark renderer
---

# Troubleshooting Markdown Issues After Updating Hugo Static Website Generator

## Introduction:
Hugo, the popular static website generator, offers an efficient and reliable way to create websites. However, like any software, it occasionally encounters issues that require troubleshooting. One such problem arises when updating the Hugo binary, causing markdown code, including HTML snippets, to malfunction. This issue can disrupt various aspects of your website, such as the display of Google AdSense ads and the application of custom styling using HTML tags. In this blog post, we will explore a potential solution to this problem by editing the config.toml file.

## Understanding the Issue:
After updating the Hugo binary, you may notice that your markdown code is not rendering as expected. HTML tags, which you used for various purposes like formatting and embedding Google AdSense ads, may no longer work as intended. This can lead to a loss of functionality and aesthetic appeal on your website.

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

## Addressing the Issue:
Fortunately, there is a straightforward solution to this problem. By editing the config.toml file, we can instruct Hugo to allow the rendering of unsafe content, including raw HTML tags, within markdown files. Here's how you can do it:

Locate the config.toml file in the root directory of your Hugo project.
Open the file using a text editor of your choice.
Editing the config.toml File:
Within the config.toml file, you will find various settings that govern the behavior of your Hugo website. To enable the rendering of unsafe content, add the following lines to the file:

```
[markup.goldmark.renderer]
  unsafe = true
```

By setting unsafe to true under the [markup.goldmark.renderer] section, we explicitly allow the rendering of potentially unsafe content. This modification ensures that your HTML code, including Google AdSense ads and custom styling tags, will work properly within markdown files.

Save the config.toml file after making the changes and then restart your Hugo server or rebuild your website to apply the updated configuration.
