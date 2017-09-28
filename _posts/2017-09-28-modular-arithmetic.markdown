---
layout: post
title:  "Modular arithmetic"
date:   2017-09-28 11:53:9 +0900
categories: algorithm
---
```
(a + b) % M = ((a % M) + (b % M)) % M

(a - b) % M = ((a % M) - (b % M)) % M

(a * b) % M = ((a % M) * (b % M)) % M

(a / b) % M = (a * modInv(b, M)) % M

modInv(b, M) = b^(m-2) % M (Fermat's Little Theorem)
```
