---
layout: post
title: MultiFasta to Fasta
---

Convert a multifasta file to a fasta file with a single header

```
grep -v "^>" /path/to/multifasta.fa | awk 'BEGIN { ORS=""; print ">header_name\n" } { print }' > /path/to/singlefasta.fa
```
