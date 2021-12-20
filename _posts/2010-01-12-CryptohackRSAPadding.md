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
So to get an output it seems you have to run "nc socket.cryptohack.org 13386" <br>
and then send {  "option" : "get_flag" }. You can run this many times on the same connection and get different values for <br>
encrypted_flag (c) and padding, but the same value for modulus (n). If you disconnect and reconnect you'll get a different modulus. 

In the script, we see e = 11. We are given a and b for padding (which changes). Messages are encrypted as <br>
c1 = (a1 * x + b1)<sup>11</sup> mod n <br>
c2 = (a2 * x + b2)<sup>11</sup> mod n <br>
etc

To my surprise, I found that the Coppersmith theorem is applicable to affine padding (https://www.utc.edu/sites/default/files/2021-04/course-paper-5600-rsa.pdf).

Now, let 

g1(x, y) = 
g2(x, y) = 


*unfinished*

