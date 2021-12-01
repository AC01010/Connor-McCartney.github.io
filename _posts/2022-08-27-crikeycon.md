---
title: Crikeycon CTF
categories:
- Crikeycon
excerpt: |
  
---



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


<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
And we find the password! "starwars"

### Key recovery
Given: <br>
pub.key 
```
-----BEGIN PUBLIC KEY-----
MDQwDQYJKoZIhvcNAQEBBQADIwAwIAIZAK78XjKNRUjBVAar1kk063HcnFuZyzpp
fwIDAQAB
-----END PUBLIC KEY-----
```

message
```
e$bæ9gxœáÔCÚ`ÎNÒÑ œ,&
```

SOLUTION

Run "openssl rsa -text -pubin -in pub.key" <br>
Exponent: 65537 (0x10001) <br>
Modulus: <br>
    00:ae:fc:5e:32:8d:45:48:c1:54:06:ab:d6:49:34: <br>
    eb:71:dc:9c:5b:99:cb:3a:69:7f <br>
Convert modulus to decimal:
```python
n = "00:ae:fc:5e:32:8d:45:48:c1:54:06:ab:d6:49:34:eb:71:dc:9c:5b:99:cb:3a:69:7f"
print(int(n.replace(":", ""), 16))
```

