---
title: HASH FUNCTION - Probability
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Hash Functions challenges](https://cryptohack.org/challenges/hashes/).

### Jack's Birthday Hash

Bit array length = 11 <br>
p = probability of collision = 0.5

Looking on [the Birthday Problem Wikipedia](https://en.wikipedia.org/wiki/Birthday_problem) I found the following formula under "Same birthday as you":

p = 1 - ((d - 1)/d)<sup>n<sup>

  <mfrac>1 3<mfrac>
