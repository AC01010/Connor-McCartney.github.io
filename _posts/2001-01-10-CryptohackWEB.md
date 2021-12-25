---
title: CRYPTO ON THE WEB - JSON Web Tokens
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Crypto On The Web challenges](https://cryptohack.org/challenges/web/).

### Token Appreciation

I decoded the token with [https://jwt.io/](https://jwt.io/) to get crypto{jwt_contents_can_be_easily_viewed}.


### JWT Sessions

What is the name of the HTTP header used by the browser to send JWTs to the server? <br>
authorization

### No Way JOSE

To solve this challenge we create a JWT using the 'none' algorithm. 

```python
import jwt #pip install pyjwt
print(jwt.encode({'username':'','admin':'true'},'',algorithm='none'))
```
