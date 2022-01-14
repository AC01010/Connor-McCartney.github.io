---
title: HASH FUNCTION - Collisions
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Hash Functions challenges](https://cryptohack.org/challenges/hashes/).

### Collider


Connect at nc socket.cryptohack.org 13389. I googled MD5 hash collisions and found this pair. Send both:

```
{"document": "4dc968ff0ee35c209572d4777b721587d36fa7b21bdc56b74a3dc0783e7b9518afbfa200a8284bf36e8e4b55b35f427593d849676da0d1555d8360fb5f07fea2"}
```

```
{"document": "4dc968ff0ee35c209572d4777b721587d36fa7b21bdc56b74a3dc0783e7b9518afbfa202a8284bf36e8e4b55b35f427593d849676da0d1d55d8360fb5f07fea2"}
```
