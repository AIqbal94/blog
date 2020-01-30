---
layout: post
title: Copying files from a remote server to local machine using rsync
---

To copy/sync files and directories from a remote server to your local machine type in the terminal of your local machine;

```
rsync -rtvh UName@server_name:/path/to/directory/to/sync/ /path/to/destination/
```
