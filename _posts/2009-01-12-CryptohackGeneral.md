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
