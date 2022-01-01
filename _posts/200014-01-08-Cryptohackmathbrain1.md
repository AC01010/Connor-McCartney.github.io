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

c1 = (2p)<sup>e1</sup> + (3q)<sup>e1</sup> mod N <br>
c2 = (5p)<sup>e1</sup> + (7q)<sup>e2</sup> mod N 

Raise c1 to the power of e2:

(c1)<sup>e2</sup> = ((2p)<sup>e1</sup> + (3q)<sup>e1</sup>)<sup>e2</sup> mod N <br>
(c1)<sup>e2</sup> = (2p)<sup>e1.e2</sup> + (3q)<sup>e1.e2</sup> mod N (middle terms cancel again) <br>
(c1)<sup>e2</sup> = (2)<sup>e1.e2</sup>(p)<sup>e1.e2</sup> + (3)<sup>e1.e2</sup>(q)<sup>e1.e2</sup> mod N

Raise c2 to the power of e1:

(c2)<sup>e1</sup> = ((5p)<sup>e2</sup> + (7q)<sup>e2</sup>)<sup>e1</sup> <br>
(c2)<sup>e1</sup> = (5p)<sup>e1.e2</sup> + (7q)<sup>e1.e2</sup> mod N (middle terms cancel again)<br>
(c2)<sup>e1</sup> = (5)<sup>e1.e2</sup>(p)<sup>e1.e2</sup> + (7)<sup>e1.e2</sup>(q)<sup>e1.e2</sup> mod N

Now we are able to eliminate either (p)<sup>e1.e2</sup> or (q)<sup>e1.e2</sup>. I shall do (q)<sup>e1.e2</sup>.

(3)<sup>-e1.e2</sup>(c1)<sup>e2</sup> = (3)<sup>-e1.e2</sup>(2)<sup>e1.e2</sup>(p)<sup>e1.e2</sup> + (q)<sup>e1.e2</sup> mod N <br>
(7)<sup>-e1.e2</sup>(c2)<sup>e1</sup> = (7)<sup>-e1.e2</sup>(5)<sup>e1.e2</sup>(p)<sup>e1.e2</sup> + (q)<sup>e1.e2</sup> mod N
