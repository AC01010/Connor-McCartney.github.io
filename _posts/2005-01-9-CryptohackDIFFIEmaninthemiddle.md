---
title: DIFFIE-HELLMAN - Man in the Middle
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Diffie-Hellman challenges](https://cryptohack.org/challenges/diffie-hellman/).

### Parameter Injection

Connect at nc socket.cryptohack.org 13371.

First send Alice's info to Bob. Then send the following to Alice, and decrypt with shared secret = 1. 

```
{"B":"0x1"}
```

### Export-grade

We can solve this with discrete log:

```python
#send {"supported": ["DH64"]}
#send {"chosen": "DH64"}

alice = {"p": "0xde26ab651b92a129", "g": "0x2", "A": "0x39af068c2a41ee7b"}
bob = {"B": "0x24508c29f91166b3"}
flag = {"iv": "12ad22612ae8f9229068f3728dae0885", "encrypted_flag": "e4d8261efbe6c69d9fcf259e51630b0519dd4af0cb0ff7759f78a499b66a8b14"}

R = GF(alice["p"])
g = R(alice["g"])
A = R(alice["A"])
B = R(bob["B"])
n = discrete_log(A, g)
print(f"secret: {B**n}")
#crypto{d0wn6r4d35_4r3_d4n63r0u5}
```

### Static Client

Connect at nc socket.cryptohack.org 13373.

From the title of the challenge I assume Bob's private key is static. Since we have A, which is g<sup>a</sup>, if we send Bob g=A, he will compute:<br>
B = A<sup>b</sup> = g<sup>a<sup>b</sup></sup> = g<sup>ab</sup> <br>

Which is the shared secret. This can be used to decrypt the ciphertext intercepted from Alice.
  
