---
title: SYMMETRIC CIPHERS - Symmetric Starter
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Symmetric Ciphers challenges](https://cryptohack.org/challenges/aes/).

### Modes of Operation Starter

Click 'encrypt flag' to get the ciphertext: <br>
74448bae417a30287706648f00bfc311e36ac2c636a27282ae2edf37a0487788

Copy paste the ciphertext into decrypt to get: <br>
63727970746f7b626c30636b5f633170683372355f3472335f663435375f217d

Convert from hex to get: <br>
crypto{bl0ck_c1ph3r5_4r3_f457_!}

### Passwords as Keys

Download word list with 
<br>
```
wget https://gist.githubusercontent.com/wchargin/8927565/raw/d9783627c731268fb2935a731a618aa8e95cf465/words
```
Now bruteforce all words.
<br>
```python
import hashlib
from Crypto.Cipher import AES

def decrypt(ciphertext, password_hash):
    ciphertext = bytes.fromhex(ciphertext)
    cipher = AES.new(key, AES.MODE_ECB)
    try:
        decrypted = cipher.decrypt(ciphertext)
    except ValueError as e:
        return {"error": str(e)}
    return decrypted

with open("words") as f:
    words = [w.strip() for w in f.readlines()]

ciphertext = "c92b7734070205bdf6c0087a751466ec13ae15e6f1bcdd3f3a535ec0f4bbae66"
for keyword in words:
    key = hashlib.md5(keyword.encode()).digest()
    decrypted = decrypt(ciphertext, key)
    if b"crypto" in decrypt(ciphertext, key):
        print(decrypted.decode())
        break
#crypto{k3y5__r__n07__p455w0rdz?}
```

