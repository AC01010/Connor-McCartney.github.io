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

Using sage:

```python
A = matrix([[4,1,3,-1], [2,1,-3,4], [1,0,-2,7], [6, 2, 9, -5]])
x = A.gram_schmidt()[0][3][1]
print(round(x, 5))
#0.91611
```

### What's a Lattice?

Using sage:

```python
A=matrix(3,3,[6, 2, -3, 5, 1, 4, 2, 7, 1])
print(abs(A.det()))
#255
```

### Gaussian Reduction

Using sage:

```python
def gauss_reduction(v1,v2):
    while true:
        if v2.norm() < v1.norm():
            tmp = v1
            v1 = v2
            v2 = tmp
        m = floor(v1.dot_product(v2) / v1.dot_product(v1))
        if m == 0:
            return v1,v2
        v2 = v2 - m * v1

v1,v2 = gauss_reduction(vector([846835985, 9834798552]),vector([87502093, 123094980]))
print(v1*v2)
#7410790865146821
```


