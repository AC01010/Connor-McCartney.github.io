---
title: RSA - Signatures Part 1
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack RSA challenges](https://cryptohack.org/challenges/rsa).


### Signing Server

Connect at nc socket.cryptohack.org 13374. <br>
Send: <br>
```
{"option":"get_secret"}
```
