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
x<sup>1/2</sup> mod p = x<sup>(p+1)/4 mod p

Now we can try just calculating x<sup>(p+1)/4 mod p and if it gives an integer that is our answer. 
```python
p = 101524035174539890485408575671085261788758965189060164484385690801466167356667036677932998889725476582421738788500738738503134356158197247473850273565349249573867251280253564698939768700489401960767007716413932851838937641880157263>
ints = [25081841204695904475894082974192007718642931811040324543182130088804239047149283334700530600468528298920930150221871666297194395061462592781551275161695411167049544771049769000895119729307495913024360169904315078028798025169985>
for a in ints:
        if pow(a, (p-1)//2, p) == 1:
                x = a
#found quadratic residue x
print(pow(x, (p+1)//4, p))
```
