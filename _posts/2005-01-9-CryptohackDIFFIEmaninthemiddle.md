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

