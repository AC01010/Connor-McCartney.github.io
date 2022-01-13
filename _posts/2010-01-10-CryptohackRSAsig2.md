---
title: RSA - Signatures Part 2
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack RSA challenges](https://cryptohack.org/challenges/rsa).


### Vote for Pedro

```python
bytes2long = lambda x: int.from_bytes(x, 'big')
x = mod(bytes2long(b"VOTE FOR PEDRO"), 2**120).nth_root(3)
print('{' + f'"option":"vote","vote":"{hex(x)[2:]}"' + '}')
#{"option":"vote","vote":"a4c46bfb65e7eccc4e76a1ce2afc6f"}
```
