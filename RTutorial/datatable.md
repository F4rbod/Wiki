---
title: data.table package
description: 
published: true
date: 2023-05-14T14:14:47.310Z
tags: 
editor: markdown
dateCreated: 2023-05-14T00:35:12.494Z
---

# Header
Your content here

![0add88a0-b4a2-433d-94ec-3ab65f8b498a_1_105_c.jpeg](/images/0add88a0-b4a2-433d-94ec-3ab65f8b498a_1_105_c.jpeg)


```r
print("hello world")
library(data.table)
a <- fread("aa.csv")
head(a)
a[, .(mean = mean(x), sd = sd(x)), by = .(y)]
```
