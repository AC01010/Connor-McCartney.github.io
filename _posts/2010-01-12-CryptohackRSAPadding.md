---
title: RSA - Padding
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack RSA challenges](https://cryptohack.org/challenges/rsa).


### Bespoke Padding

Given:
```python
from utils import listener
from Crypto.Util.number import bytes_to_long, getPrime
import random

FLAG = b'crypto{???????????????????????????}'


class Challenge():
    def __init__(self):
        self.before_input = "Come back as much as you want! You'll never get my flag.\n"
        self.p = getPrime(1024)
        self.q = getPrime(1024)
        self.N = self.p * self.q
        self.e = 11

    def pad(self, flag):
        m = bytes_to_long(flag)
        a = random.randint(2, self.N)
        b = random.randint(2, self.N)
        return (a, b), a*m+b

    def encrypt(self, flag):
        pad_var, pad_msg = self.pad(flag)
        encrypted = (pow(pad_msg, self.e, self.N), self.N)
        return pad_var, encrypted

    def challenge(self, your_input):
        if not 'option' in your_input:
            return {"error": "You must send an option to this server"}

        elif your_input['option'] == 'get_flag':
            pad_var, encrypted = self.encrypt(FLAG)
            return {"encrypted_flag": encrypted[0], "modulus": encrypted[1], "padding": pad_var}

        else:
            return {"error": "Invalid option"}


"""
When you connect, the 'challenge' function will be called on your JSON
input.
"""
listener.start_server(port=13386)
```

SOLUTION

At the bottom of their script it says the challenge function is called on your JSON input. 
So to get an output run:
<br>
```
nc socket.cryptohack.org 13386
```
and then send :
<br>
```
{"option" : "get_flag"}
```
You can run this many times on the same connection and get different values for encrypted_flag (c) and padding, but the same value for modulus (n). If you disconnect and reconnect you'll get a different modulus too. 

In the script, we see e = 11. We are given a and b for padding (which changes). Messages are encrypted as <br>
c1 = (a1 * x + b1)<sup>11</sup> mod n <br>
c2 = (a2 * x + b2)<sup>11</sup> mod n <br>
etc

To my surprise, I found that the Coppersmith theorem is applicable to affine padding (https://www.utc.edu/sites/default/files/2021-04/course-paper-5600-rsa.pdf). <br>
We can use the Franklin-Reiter attack, here is my sage implmentation:

```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a.monic()

def FranklinReiter(c1, c2, e, n, a1, b1, a2, b2):
    P.<X> = PolynomialRing(Zmod(n))
    g1 = (a1*X + b1)^e - c1
    g2 = (a2*X + b2)^e - c2
    return int(-gcd(g1, g2).coefficients()[0])

long2bytes = lambda x: x.to_bytes((x.bit_length() + 7) // 8, 'big')
m = FranklinReiter(c1, c2, e, n, a1, b1, a2, b2)
print(long2bytes(m).decode())
```

This gives crypto{linear_padding_isnt_padding}.

### Null or Never

Given: <br>
```python
    from Crypto.PublicKey import RSA
    from Crypto.Util.number import bytes_to_long
    FLAG = b"crypto{???????????????????????????????????}"
    def pad100(msg):
        return msg + b'\x00' * (100 - len(msg))
    key = RSA.generate(1024, e=3)
    n, e = key.n, key.e
    m = bytes_to_long(pad100(FLAG))
    c = pow(m, e, n)
    print(f"n = {n}")
    print(f"e = {e}")
    print(f"c = {c}")
```
pad100 adds zeros to the end of the flag until the length is 100 bytes. Appending one zero to the end (1 byte) is equivalent to multiplying the plaintext by 2<sup>8</sup>. <br>
The flag length is 43, so 57 zeros were appended. Thus the plaintext was encrypted as: <br>
c = (m * 2<sup>8 * 57</sup>)<sup>3</sup> mod n <br>
Solving for m: <br>
c = (m<sup>3</sup> * 2<sup>8 * 57 * 3</sup>) mod n <br>
m<sup>3</sup> = (c * 2<sup>8 * 57 * 3</sup>) mod n <br>
