---
title: "Hugo Tag Page Does Not Work"
date: 2023-06-24T07:32:02-07:00
categories:
  - Technology
tags:
  - Hugo
  - Tags
---

I recently encountered an issue with tag pages not working properly in Hugo, a popular static site generator. Whenever I clicked on a tag link, instead of being directed to the corresponding tag page, I was redirected to a blank page served by hugo. Frustrating, right?

After some investigation, I found a simple workaround to fix this problem: restarting the Hugo server. It seems that the issue stems from a caching or routing glitch within Hugo.

To resolve the problem, follow these steps:

1. Stop the Hugo server by pressing `Ctrl + C` in the terminal where the server is running.
2. Restart the Hugo server by running the command `hugo server` again.
3. Once the server is up and running, navigate to the tag pages and click on the tags. Voila! They should now work as expected.

I hope this helps anyone else who might be facing the same issue.