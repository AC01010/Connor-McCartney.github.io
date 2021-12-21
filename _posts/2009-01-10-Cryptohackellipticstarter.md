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

