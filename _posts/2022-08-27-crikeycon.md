---
title: Crikeycon CTF
categories:
- Crikeycon
excerpt: |
  
---


### Key recovery
Given: <br>
pub.key 
```
-----BEGIN PUBLIC KEY-----
MDQwDQYJKoZIhvcNAQEBBQADIwAwIAIZAK78XjKNRUjBVAar1kk063HcnFuZyzpp
fwIDAQAB
-----END PUBLIC KEY-----
```

message_hexdump
```
00000000: 6524 62e6 3967 78bd e1d4 4388 97da 60ce  e$b.9gx...C...`.
00000010: 4ed2 d1a0 bd2c 2616                      N....,&.
```

SOLUTION

To create the message, run "xxd -r x > message"

message
```
e$bæ9gxœáÔCÚ`ÎNÒÑ œ,&
```


Run "openssl rsa -text -pubin -in pub.key" <br>
Exponent: 65537 (0x10001) <br>
Modulus: 00aefc5e328d4548c15406abd64934eb71dc9c5b99cb3a697f <br>

Convert modulus to decimal:
```python
n = "00:ae:fc:5e:32:8d:45:48:c1:54:06:ab:d6:49:34:eb:71:dc:9c:5b:99:cb:3a:69:7f"
print(int(n.replace(":", ""), 16))
```
This gives n = 4290639633996267716557385332009265040461674354008108198271. <br>
Factoring with http://factordb.com/ gives p = 63539306977486994912339790113 and <br>
q = 67527328170521450591465143967. <br>

Then I generated the public key. 
```python
import pyasn1.codec.der.encoder #pip install pyasn1
import pyasn1.type.univ
import base64

n = 4290639633996267716557385332009265040461674354008108198271
e = 65537
p = 63539306977486994912339790113
q = 67527328170521450591465143967
phi = (p-1)*(q-1)
d = pow(e, -1, phi)

#https://crypto.stackexchange.com/questions/25498/how-to-create-a-pem-file-for-storing-an-rsa-key/25499#25499
dP = d % (p-1)
dQ = d % (q-1)
qInv = pow(q, p-2, p)

def pempriv(n, e, d, p, q, dP, dQ, qInv):
    template = '-----BEGIN RSA PRIVATE KEY-----\n{}-----END RSA PRIVATE KEY-----\n'
    seq = pyasn1.type.univ.Sequence()
    for i,x in enumerate((0, n, e, d, p, q, dP, dQ, qInv)):
        seq.setComponentByPosition(i, pyasn1.type.univ.Integer(x))
    der = pyasn1.codec.der.encoder.encode(seq)
    return template.format(base64.encodebytes(der).decode('ascii'))


print(pempriv(n, e, d, p, q, dP, dQ, qInv))
```

I saved this as priv.key
```
-----BEGIN RSA PRIVATE KEY-----
MIGHAgEAAhkArvxeMo1FSMFUBqvWSTTrcdycW5nLOml/AgMBAAECGE2ZXxCd83azPVcEBnR7xuCr
ITHTBSxMQQINAM1Oe50pJii/6FOJIQINANoxS/M7JfZ+ZJp+nwIMDXrctOnxvnUzPODhAg0AmlRy
BynYEUCXQhu7Ag0AnZHcrWk2J5k5z8ym
-----END RSA PRIVATE KEY-----
```
Then decrypted by running "cat message | openssl rsautl -decrypt -inkey priv.key". <br>
This gave flag{gudk1d}.

### Billy is a Shadow of his Former Self

Help Billy become the person he used to be before hash got control of him.
user1:$6$KbmN9bCSWQK2.h5o$ExR7Lh4OeLBP3173vBPWmgp/QHlaQu3U/PcNURnHalelr/l34hw.2kifKIPNkUdhiLk/f7HTaKhS2DACSCXJz/:18660:0:99999:7:::

SOLUTION

The password hash format for /etc/shadow file fields is $id$salt$hashed. <br>
So id is 6, <br>
salt is KbmN9bCSWQK2.h5o, <br>
and hash is ExR7Lh4OeLBP3173vBPWmgp/QHlaQu3U/PcNURnHalelr/l34hw.2kifKIPNkUdhiLk/f7HTaKhS2DACSCXJz/

id is 6 means it is a SHA-512 hash. We can pass the whole hash into hashcat:

{% include figure.html image="https://i.postimg.cc/dQ4mp6H7/hashcat1.png" position="left" width="1206" height="526" %}

{% include figure.html image="https://i.postimg.cc/Yq0fHFZd/hashcat2.png" position="left" width="1064" height="733" %}


<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
And we find the password! "starwars"
