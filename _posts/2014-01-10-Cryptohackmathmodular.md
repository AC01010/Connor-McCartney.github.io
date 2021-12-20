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

We are given a is a quadratic residue if a<sup>(p-1)/2</sup> mod p = 1. <br>
Once we have found the quadratic residue x, we need the square root. That is, <br>
x<sup>1/2</sup> mod p <br>

We know 1 = x<sup>(p-1)/2</sup> mod p <br>
Multiply both sides by x: <br>
x = x * x<sup>(p-1)/2</sup> mod p <br>
x = x<sup>(p+1)/2 mod p <br>
We can sub this value for x mod p in:
x<sup>1/2</sup> mod p <br> = x<sup>(p+1)/4 mod p









We can apply Fermat's Little Theorem: x<sup>p-1</sup> = 1 mod p  <br>
