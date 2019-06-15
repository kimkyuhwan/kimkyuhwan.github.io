---
layout: post
title:  "Suffix Array & LCP"
date:   2019-06-16 00:00:00 0000
author : "Gyuhwan Kim"
categories: Algorithm suffix-array LCP
use_math: true
---

## Suffix Array Algorithm

##### Fact 1.

$ lcp(Pos[y-1],Pos[y]) \geq lcp(Pos[x],Pos[z]), x \leq y \leq z $

##### Fact 2.

$ \mathbf{if} \: lcp(Pos[x-1], Pos[x]) > 1, \mathbf{then} \: Rank[Pos[x-1]+1] < Rank[Posx]+1] $

##### Fact 3

$ \mathbf{if} \: lcp[Pos[x-1], Pos[x]] > 1, \mathbf{then} \: lcp(Pos[x-1]+1, Pos[x]+1)=lcp(Pos[x-1],Pos[x])-1 $

