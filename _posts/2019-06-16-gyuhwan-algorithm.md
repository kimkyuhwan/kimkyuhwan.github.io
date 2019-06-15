---
layout: post
title:  "Suffix Array & LCP"
date:   2019-06-16 00:00:00 0000
author : "Gyuhwan Kim"
categories: Algorithm suffix-array LCP
use_math: true
---

## Suffix Array Algorithm

$ Fact1. lcp(Pos[y-1],Pos[y]) \req lcp(Pos[x],Pos[x']), x \leq y \leq x' $