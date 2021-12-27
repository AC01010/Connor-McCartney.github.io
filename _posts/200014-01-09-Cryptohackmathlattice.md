---
title: MATHEMATICS - Lattices
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Mathematics challenges](https://cryptohack.org/challenges/maths/).

### Vectors

Using sage:

```python
v, w, u = vector( (2,6,3) ), vector( (1,0,0) ), vector( (7,7,2) )
print((3*(2*v - w)).dot_product(2*u))
#702
```

### Size and Basis

Using sage:

```python
v = vector( (4,6,2,5) )
print(sqrt(v.dot_product(v)))
#9
```

### Gram Schmidt
