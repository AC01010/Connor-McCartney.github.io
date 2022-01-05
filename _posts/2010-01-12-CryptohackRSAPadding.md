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
<br>
and then send :
<br>
```
{"option" : "get_flag"}
```
<br>
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

c1 = 66854333624806319889680995186065628952573290741161017502568304869718711230606370377057302135063444382705416329432513508306590414246716689432121764089058141350762>
c2 = 20071101238189004470631301594649282716276651543347436391273341610560131064705626600588213601909548534276865432714239803372364439421506882877403365992818490474073>
e = 11
n = 232418106760282746147924976981643926031223547445574451740622178232979653051626617733323722433249806490553263542850658012859315448409337734612139168997397305223892>
a1 = 95976116312631923679164905806409895619750799367526977290071106825747937451599461602406007808609707685103118350765189835916263000816190827957785643368896276347953>
b1 = 17071185027969852273714435176457822330543369520858286465151793657514023465241447840640931821878068240162290446364788674165900738392197530666520576029069935293001>
a2 = 34642474487778928703010345302097682050011091400508681353271058839491580526190724380634565334663156531291444014269384055902430708189659475582148705725624379056207>
b2 = 34122231518635051120850647295550984464587778958446654602054289075195933043455768336682693437479326090376184632156696545233619211023414177172286596927374055432622>

long2bytes = lambda x: x.to_bytes((x.bit_length() + 7) // 8, 'big')
m = FranklinReiter(c1, c2, e, n, a1, b1, a2, b2)
print(long2bytes(m).decode())
```

This gives crypto{linear_padding_isnt_padding}.
