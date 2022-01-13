---
title: MATHEMATICS - Brainteasers Part 2
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Mathematics challenges](https://cryptohack.org/challenges/maths/).



### Unencryptable

This challenge was a matter of bruteforcing to factorise N using gcd. 

```python
from math import gcd
from Crypto.Util.number import long_to_bytes
N = 0x7fe8cafec59886e9318830f33747cafd200588406e7c42741859e15994ab62410438991ab5d9fc94f386219e3c27d6ffc73754f791e7b2c565611f8fe5054dd132b8c4f3eadcf1180cd8f2a3cc756b06996f2d5b67c390adcba9d444697b13d12b2badfc3c7d5459df16a047ca25f4d18570cd6fa727aed46394576cfdb56b41
e = 0x10001
c = 0x5233da71cc1dc1c5f21039f51eb51c80657e1af217d563aa25a8104a4e84a42379040ecdfdd5afa191156ccb40b6f188f4ad96c58922428c4c0bc17fd5384456853e139afde40c3f95988879629297f48d0efa6b335716a4c24bfee36f714d34a4e810a9689e93a0af8502528844ae578100b0188a2790518c695c095c9d677b
m =0x372f0e88f6f7189da7c06ed49e87e0664b988ecbee583586dfd1c6af99bf20345ae7442012c6807b3493d8936f5b48e553f614754deb3da6230fa1e16a8d5953a94c886699fc2bf409556264d5dced76a1780a90fd22f3701fdbcb183ddab4046affdc4dc6379090f79f4cd50673b24d0b08458cdbe509d60a4ad88a7b4e2921

for i in range(1, 16):
    if gcd(m ** (2 ** i) - 1, N) != 1:
        p = gcd(m ** (2 ** i) - 1, N)
        break
q = N // p
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
m = pow(c, d, N)
print(long_to_bytes(m).decode())
#crypto{R3m3mb3r!_F1x3d_P0iNts_aR3_s3crE7s_t00}
```

### Cofactor Cofantasy

This challenge boils down to distinguising if a given number c is either: <br>
a) random <br>
b) equal to g<sup>x</sup> mod N, for some random integer x

I started by finding divisors of phi to work with. 

```python
for i in range(1000):
    if phi % (2**i) == 0:
        print(i)
```

The largest I found was 2<sup>16</sup>. Now let M = phi//(2 ** 16). <br>

Next I looked at g<sup>M</sup> mod N and g<sup>2M</sup> mod N:

```python
print(pow(g, M, N)) # does not equal 1
print(pow(g, 2*M, N)) # equals 1
```

Let k = g<sup>M</sup> mod N. The fact that we know N is a product of distinct primes and that k is a square root of 1 mod N
allows us to factor N:

```python
from math import gcd
M = phi//(2 ** 16)
k = pow(g, M, N)
a = gcd(N, k+1)
b = gcd(N, k-1)
```

