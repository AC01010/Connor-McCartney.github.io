---
title: MATHEMATICS - BRAINTEASERS PART 1
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Mathematics challenges](https://cryptohack.org/challenges/maths/).

### Modular Binomials

c1 = (2p + 3q)<sup>e1</sup> mod N <br>
c2 = (5p + 7q)<sup>e2</sup> mod N

Expand the binomial and the middle terms are multiples of pq so they cancel under the mod pq.

c1 = 2p<sup>e1</sup> + 3q<sup>e1</sup> mod N <br>
c2 = 5p<sup>e1</sup> + 7q<sup>e2</sup> mod N 

Raise c1 to the power of e2:



