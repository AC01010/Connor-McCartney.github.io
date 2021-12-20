---
title: MATHEMATICS - Modular Math
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Mathematics challenges](https://cryptohack.org/challenges/maths/).

### Quadratic Residues

Given:
```python
p = 29
ints = [14, 6, 11]
```

We have to find which of the ints is a quadratic residue. <br>
x is a Quadratic Residue if there exists an a such that a<sup>2</sup> = x mod p.

```python
p = 29
ints = [14, 6, 11]
for x in ints:
        for a in range(p):
                if a**2 % p == x:
                        print(a) #8, 21, min is 8
```

### Legendre Symbol
