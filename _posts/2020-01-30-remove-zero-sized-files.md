---
layout: post
title: Remove zero sized files from Linux
---

To remove multiple zero sized files from current directory in linux at once use;

```
find . -size 0 -delete
```
