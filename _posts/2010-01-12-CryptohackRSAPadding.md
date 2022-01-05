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
m<sup>3</sup> = (c * 2<sup>-8 * 57 * 3</sup>) mod n <br>
Let x = (c * 2<sup>-8 * 57 * 3</sup>) mod n <br>
m<sup>3</sup> = x + kn <br>
m = cbrt(x + kn)

Solve script:
```python
from gmpy2 import iroot
long2bytes = lambda x: x.to_bytes((x.bit_length() + 7) // 8, 'big')
def nth_root(x,n):
    high = 1
    while high ** n <= x:
        high *= 2
    low = high // 2
    while low < high:
        mid = int((low + high) // 2) + 1
        if low < mid and mid ** n < x:
            low = mid
        elif high > mid and mid ** n > x:
            high = mid
        else:
            return mid
    return mid + 1

n=95341235345618011251857577682324351171197688101180707030749869409235726634345899397258784261937590128088284421816891826202978052640992678267974129629670862991769812330793126662251062120518795878693122854189330426777286315442926939843468730196970939951374889986320771714519309125434348512571864406646232154103
c=63476139027102349822147098087901756023488558030079225358836870725611623045683759473454129221778690683914555720975250395929721681009556415292257804239149809875424000027362678341633901036035522299395660255954384685936351041718040558055860508481512479599089561391846007771856837130233678763953257086620228436828
x  = ((c * pow(2, -8*57*3,n))) % n
for k in range(100):
    m = nth_root(x + k*n, 3)
    if b"crypto" in long2bytes(m):
        print(long2bytes(m).decode())
        break
#crypto{n0n_574nd4rd_p4d_c0n51d3r3d_h4rmful}
```
