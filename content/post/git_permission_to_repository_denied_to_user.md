---
title: "Permission to Repository Denied to Git User"
date: 2023-06-09T20:38:03-07:00
draft: true
categories:
  - Software Development
  - Technology
tags:
  - Git
  - Permission denied
  - Error 403
---

# Permission to Repository Denied to Git User

If you are reading this post because you've already tried a few solutions out there
including switching from https to ssh in your git config or verifying your repository
access, then continue reading.

Most likely you need to clear your Git credentials.

- For Windows:
Open the Credential Manager and remove any Git-related credentials.
Open a command prompt and run the following command to clear the Git credential cache:
```
git config --global --unset credential.helper
```

- For macOS/Linux:
Open the Keychain Access (macOS) or Credential Manager (Linux) and remove any Git-related credentials.
Open a terminal and run the following command to clear the Git credential cache:
```
git config --global --unset credential.helper
```

If you are note a big fan of CLI, on mac you can open Keychain Access,
search for github.com and delete the entry.

Hope this saves you some time.
