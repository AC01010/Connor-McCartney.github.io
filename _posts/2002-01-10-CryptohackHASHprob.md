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

p = 1 - ((d - 1)/d)<sup>n</sup>

Now rearrange for n:

((d - 1)/d)<sup>n</sup> = 1 - p<br>
n = log(1-p) / log((d - 1)/d)

Now substitute in p = 0.5 and d = 2<sup>11</sup>

```python
from math import log
p = 0.5
d = 2**11
print(log(1-p) / log((d-1)/d))
```
### Jack's Birthday Confusion

Bit array length = 11 <br>
p = probability of collision = 0.75

Looking on [the Birthday Problem Wikipedia](https://en.wikipedia.org/wiki/Birthday_problem) I found the following formula under "Probability of a shared birthday (collision)":

n = sqrt(2d * ln(1/(1-p)))

```python
p = 0.75
d = 2**11
print(sqrt(2*d*log(1/(1-p))))
#76
```
