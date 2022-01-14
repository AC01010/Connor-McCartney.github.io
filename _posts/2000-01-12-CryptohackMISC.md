---
title: MISC
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Misc challenges](https://cryptohack.org/challenges/misc/).

### Gotta Go Fast

Connect at nc socket.cryptohack.org 13372.

We can get an encrypted flag:

```
{"option": "get_flag"}
```

This otb is just xor, so we can decrypt by encrypting:

```
{"option": "encrypt_data", "input_data": "FLAG_HERE"}
```

However, as per the title, we have to do this instantly because key = long_to_bytes(current_time).

```python
import telnetlib
import json
HOST = "socket.cryptohack.org"
PORT = 13372
r = telnetlib.Telnet(HOST, PORT)
def readline():
    return r.read_until(b"\n")
def json_recv():
    line = readline()
    return json.loads(line.decode())
def json_send(hsh):
    request = json.dumps(hsh).encode()
    r.write(request)

print(readline())
json_send({"option": "get_flag"})
json_send({"option": "encrypt_data","input_data": json_recv()["encrypted_flag"]})
print(bytes.fromhex(json_recv()["encrypted_data"]))
#crypto{t00_f4st_t00_furi0u5}
```

### Bruce Schneier's Password

A solution to Bruce 2 is a solution to this.

### Bruce Schneier's Password: Part 2

Send:
```
{"password":"kkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS7777777777777777777777777777777777777777777777wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa5555555555555555555555555555555555555555ssssssssssIIIIIIIIIIIIIIIIIIIIIIIIIIIIyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy111111111111111111111111111111111111CCCCCCCCCMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccciiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiii333333333qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqAAAAAAAAAAAOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOQQQQQQQQQQQQ99999999999999999999999999999999999999999999999999999mmmmmKKKKKKKuuuuuuuuu______UUUUUUUUUUUUUUUUUUWWWWWWWWWWWWWWWWEEEEEEEEEEEEEEEE"}
```
