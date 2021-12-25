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
print(jwt.encode({'username':'user','admin':'true'},'',algorithm='none'))
```

### JWT Secrets

To solve this challenge we use the hint # TODO: PyJWT readme key, change later. <br>
The readme key can be found at https://github.com/jpadilla/pyjwt/ and is "secret".

```python
import jwt #pip install pyjwt
print(jwt.encode({'username':'user','admin':'true'},'secret',algorithm='HS256'))
```

### JWT Secrets

We can solve this challenge by changing the signing algorithm from RS256 to HS256 and signing the token with the same public key.

```python
import jwt # pip install pyjwt==1.5.0
PRIVATE_KEY = "-----BEGIN RSA PUBLIC KEY-----\nMIIBCgKCAQEAvoOtsfF5Gtkr2Swy0xzuUp5J3w8bJY5oF7TgDrkAhg1sFUEaCMlR\nYltE8jobFTyPo5cciBHD7huZVHLtRqdhkmPD4FSlKaaX2DfzqyiZaPhZZT62w7Hi\ngJlwG7M0xTUljQ6WBiIFW9By3amqYxyR2rOq8Y68ewN000VSFXy7FZjQ/CDA3wSl\nQ4KI40YEHBNeCl6QWXWxBb8AvHo4lkJ5zZyNje+uxq8St1WlZ8/5v55eavshcfD1\n0NSHaYIIilh9yic/xK4t20qvyZKe6Gpdw6vTyefw4+Hhp1gROwOrIa0X0alVepg9\nJddv6V/d/qjDRzpJIop9DSB8qcF1X23pkQIDAQAB\n-----END RSA PUBLIC KEY-----\n"
print(jwt.encode({'username': 'user', 'admin': True}, PRIVATE_KEY, algorithm='HS256').decode())
#eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6dHJ1ZX0.kQ9Cl3pOTU1ERqB4GZi3G-4bKkHzpnycu6giuKHC0uQ
```
