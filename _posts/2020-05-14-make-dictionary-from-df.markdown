---
published: true
title: Make a dictionary from a DataFrame
layout: post
---

```python
dics=df[["ensembl_gene_id","gene_name"]]
dics["ensembl_gene_id"]=dics["ensembl_gene_id"].apply(lambda x: x.upper())
dics.index=dics["ensembl_gene_id"].tolist()
names_dic=dics[["gene_name"]].to_dict()["gene_name"]
```
