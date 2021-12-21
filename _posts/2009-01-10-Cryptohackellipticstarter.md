---
title: ELLIPTIC CURVES - Starter
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Elliptic Curves challenges](https://cryptohack.org/challenges/ecc/).

### Background Reading

The flag is the name we give groups with a commutative operation.

crypto{abelian}

### Point Negation

Given:  <br>
P(8045,6936) <br>
P + Q = O

SOLUTION

To negate the point P we must find a point directly above or below P, so that when connecting the points they form a vertical line that does not hit the curve again. Elliptic curves are symetric about the x-axis, so we can simply flip the y coordinate. <br>
Q = (8045,-6936) mod 9739 <br>
Q = (8045,2803) <br>

This gives crypto{8045,2803}.

### Point Addition

```python
E = EllipticCurve( GF(9739), [497, 1768] )
P, Q, R = E.point((493, 5564)), E.point((1539, 4742)), E.point((4403,5202))
s = (P+P+Q+R).xy()
print(s)
#(4215, 2162)
```

This gives crypto{4215, 2162}

### Scalar Multiplication

```python
E = EllipticCurve( GF(9739), [497, 1768] )
P = E.point((2339, 2213))
Q = P*7863
print(Q.xy())
#(9467, 2742)
```

This gives crypto{9467, 2742}

### Curves and Logs
