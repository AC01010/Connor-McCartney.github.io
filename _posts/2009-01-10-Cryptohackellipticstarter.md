---
title: ELLIPTIC CURVES - Starter
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Elliptic Curves challenges](https://cryptohack.org/challenges/ecc/).

### Background Reading

The flag is the name we give groups with a commutative operation.

crypto{abelian}

### Point Negation

Given:  <br>
P(8045,6936) <br>
P + Q = O

SOLUTION

To negate the point P we must find a point directly above or below P, so that when connecting the points they form a vertical line that does not hit the curve again. Elliptic curves are symetric about the x-axis, so we can simply flip the y coordinate. <br>
Q = (8045,-6936) mod 9739 <br>
Q = (8045,2803) <br>

This gives crypto{8045,2803}.

### Point Addition

```python
E = EllipticCurve( GF(9739), [497, 1768] )
P, Q, R = E.point((493, 5564)), E.point((1539, 4742)), E.point((4403,5202))
s = (P+P+Q+R).xy()
print(s)
#(4215, 2162)
```

This gives crypto{4215, 2162}

### Scalar Multiplication

```python
E = EllipticCurve( GF(9739), [497, 1768] )
P = E.point((2339, 2213))
Q = P*7863
print(Q.xy())
#(9467, 2742)
```

This gives crypto{9467, 2742}

### Curves and Logs

We are given QA = (815, 3190), nB = 1829 and must find the SHA1 hash of the x coordinate of S.

S = nA * QB = nB * QA <br>
S = QA * 1829

```python
from hashlib import sha1
E = EllipticCurve( GF(9739), [497, 1768] )
QA = E.point((815, 3190))
S = QA*1829
x = (S.xy()[0])
print(sha1(str(x).encode()).hexdigest())
#80e5212754a824d3a4aed185ace4f9cac0f908bf
```

This gives crypto{80e5212754a824d3a4aed185ace4f9cac0f908bf}

### Efficient Exchange

First we need to calculate the y coordinate by subbing the x into the curve.

```python
  print((4726**3 + 497*4726 + 1768) % 9739)
```

So y<sup>2</sup> = 5507

In the Legendre Symbol cryptohack challenge for p = 3 mod 4 we got the formula <br>
x<sup>1/2</sup> mod p = x<sup>(p+1)/4</sup> mod p

We can use this to calculate y.

```python
print(pow(5507, (9739 + 1)//4, 9739))
```

So y = 6287. Now we calculate S = QA * nB.

```python
E = EllipticCurve( GF(9739), [497, 1768] )
QA = E.point((4726, 6287))
S = QA*6534
print(S.xy())
#(1791, 2181)
```

Now enter these into decrypt.py

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import hashlib


def is_pkcs7_padded(message):
    padding = message[-message[-1]:]
    return all(padding[i] == len(padding) for i in range(0, len(padding)))


def decrypt_flag(shared_secret: int, iv: str, ciphertext: str):
    # Derive AES key from shared secret
    sha1 = hashlib.sha1()
    sha1.update(str(shared_secret).encode('ascii'))
    key = sha1.digest()[:16]
    # Decrypt flag
    ciphertext = bytes.fromhex(ciphertext)
    iv = bytes.fromhex(iv)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    plaintext = cipher.decrypt(ciphertext)

    if is_pkcs7_padded(plaintext):
        return unpad(plaintext, 16).decode('ascii')
    else:
        return plaintext.decode('ascii')


shared_secret = 1791
iv = 'cd9da9f1c60925922377ea952afc212c'
ciphertext = 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'

print(decrypt_flag(shared_secret, iv, ciphertext))
```

This gives crypto{3ff1c1ent_k3y_3xch4ng3}
