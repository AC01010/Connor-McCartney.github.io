---
title: General
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack General challenges](https://cryptohack.org/challenges/general/).


### ASCII

```python
numbers = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]
print("".join([chr(i) for i in numbers]))
#crypto{ASCII_pr1nt4bl3}
```

### Hex

```python                                                                                                             
flag = int("63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d", 16)
print(flag.to_bytes((flag.bit_length() + 7) // 8, 'big').decode())
#crypto{You_will_be_working_with_hex_strings_a_lot}
```

### Base64

```python
import base64
print(base64.b64encode(bytes.fromhex("72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf")).decode())
#crypto/Base+64+Encoding+is+Web+Safe/
```

### Bytes and Big integers

```python
flag = 11515195063862318899931685488813747395775516287289682636499965282714637259206269
print(flag.to_bytes((flag.bit_length() + 7) // 8, 'big').decode())
#crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}
```

### Encoding challenge

```python
from pwn import * # pip install pwntools
import json
import base64
import codecs

l2b = lambda x : x.to_bytes((x.bit_length() + 7) // 8, 'big').decode()

r = remote('socket.cryptohack.org', 13377, level = 'debug')

def json_recv():
    line = r.recvline()
    return json.loads(line.decode())

def json_send(hsh):
    request = json.dumps(hsh).encode()
    r.sendline(request)

while 1:
    received = json_recv()
    x = received["encoded"]

    if received["type"] == "hex":
        decoded = l2b(int(x, 16))
    elif received["type"] == "base64":
        decoded = base64.b64decode(x).decode()
    elif received["type"] == "rot13":
        decoded = codecs.decode(x, 'rot_13')
    elif received["type"] == "utf-8":
        decoded =  "".join([chr(b) for b in x])
    elif received["type"] == "bigint":
        decoded = l2b(int(x, 16))

    json_send( {"decoded": decoded})
#crypto{3nc0d3_d3c0d3_3nc0d3}
```

### XOR Starter

```python
l2b = lambda x : x.to_bytes((x.bit_length() + 7) // 8, 'big').decode()
print("".join([l2b(ord(i)^13) for i in "label"]))
#crypto{aloha}
```

### XOR Properties

```python
l2b = lambda x : x.to_bytes((x.bit_length() + 7) // 8, 'big').decode()

KEY1 = 0xa6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
KEY2 = KEY1 ^ 0x37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
KEY3 = KEY2 ^ 0xc1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1
FLAG = KEY1 ^ KEY2 ^ KEY3 ^ 0x04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf
print(l2b(FLAG))
#crypto{x0r_i5_ass0c1at1v3}
```

### Favourite byte

```python
l2b = lambda x : x.to_bytes((x.bit_length() + 7) // 8, 'big')

decoded = l2b(0x73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d)
for i in range(256):    
    flag = b''
    for b in decoded:
        flag += bytes([b ^ i])
    try:
        if "crypto" in flag.decode():
            print(flag.decode())
    except:
        pass #could not decode
#crypto{0x10_15_my_f4v0ur173_by7e}
```

### You either know, XOR you don't

```python
from pwn import xor
message = bytes.fromhex("0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104")
partial_key = xor(message[:7], b"crypto{").decode() + 'y'   #myXORkey
complete_key = (partial_key * (len(message)//len(partial_key)+1))[:len(message)] #myXORkeymyXORkeymyXORkeymyXORkeymyXORkeymy
flag = xor(message, complete_key.encode())
print(flag.decode())
```

### Lemur XOR

```python
import os
import cv2 # pip3 install opencv-python

flag = os.path.dirname(__file__) + "/flag.png"
lemur = os.path.dirname(__file__) + "/lemur.png"
result = os.path.dirname(__file__) + "/result.png"
key = cv2.bitwise_xor(cv2.imread(flag), cv2.imread(lemur))
cv2.imwrite(result, key)
#crypto{X0Rly_n0t!}
```

### Greatest Common Divisor

```python
from math import gcd
print(gcd(66528, 52920))
#1512
```

### Extended GCD

```python
def extended_gcd(p,q):
    if p == 0:
        return (q, 0, 1)
    else:
        (gcd, u, v) = extended_gcd(q % p, p)
        return (gcd, v - (q // p) * u, u)
p = 26513
q = 32321
gcd, u, v = extended_gcd(p, q)
print(f'u: {u}, v: {v}')
#-8404
```

### Modular Arithmetic 1

```python
x = 11 % 6
y = 8146798528947 % 17
print(x)
print(y)
#4
```

### Modular Arithmetic 2

```python
print(pow(273246787654, 65536, 65537))
#1
```

### Modular Inverting
```python
pow(3, -1, 13)
#9
```

### Privacy-Enhanced Mail?

```python
#alternative
#openssl rsa -in privkey.pem -text

from Crypto.PublicKey import RSA
f = open('privkey.pem','r')
key = RSA.importKey(f.read())
print(key.d)
```

### CERTainly not
```python

```
