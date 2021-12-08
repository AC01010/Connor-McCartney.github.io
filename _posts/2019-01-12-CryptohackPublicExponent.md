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
