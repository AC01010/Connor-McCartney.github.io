---
title: RSA Public Exponent
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack RSA challenges](https://cryptohack.org/challenges/rsa).


### Salty

This challenge is vulnerable to a small exponent attack. This occurs when m<sup>e</sup> is less than n. To decrypt using this attack, m = the eth root of the ciphertext.
Here e=1, so m is simply c. 
```python
from Crypto.Util.number import bytes_to_long, long_to_bytes #pip install pycryptodome

c = 44981230718212183604274785925793145442655465025264554046028251311164494127485
print(long_to_bytes(c).decode())
```
This gives crypto{saltstack_fell_for_this!}

### Modulus Inutilis

We can use small exponent attack again. 
```python
from Crypto.Util.number import long_to_bytes #pip install pycryptodome

c = 243251053617903760309941844835411292373350655973075480264001352919865180151222189820473358411037759381328642957324889519192337152355302808400638052620580409813222660643570085177957

def nth_root(x,n):
    high = 1
    while high ** n <= x:
        high *= 2
    low = high // 2
    while low < high:
        mid = int((low + high) // 2) + 1
        if low < mid and mid ** n < x:
            low = mid
        elif high > mid and mid ** n > x:
            high = mid
        else:
            return mid
    return mid + 1

print(long_to_bytes(nth_root(c, 3)).decode())
```
This gives crypto{N33d_m04R_p4dd1ng}

