---
title: CRYPTO ON THE WEB - JSON Web Tokens
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Crypto On The Web challenges](https://cryptohack.org/challenges/web/).

### Token Appreciation

I decoded the token with [https://jwt.io/](https://jwt.io/) to get crypto{jwt_contents_can_be_easily_viewed}.


### JWT Sessions

What is the name of the HTTP header used by the browser to send JWTs to the server? <br>
authorization

### No Way JOSE

To solve this challenge we create a JWT using the 'none' algorithm. 

```python
import jwt #pip install pyjwt
print(jwt.encode({'username':'user','admin':'true'},'',algorithm='none'))
```

### JWT Secrets

To solve this challenge we use the hint # TODO: PyJWT readme key, change later. <br>
The readme key can be found at https://github.com/jpadilla/pyjwt/ and is "secret".

```python
import jwt #pip install pyjwt
print(jwt.encode({'username':'user','admin':'true'},'secret',algorithm='HS256'))
```

### RSA or HMAC?

We can solve this challenge by changing the signing algorithm from RS256 to HS256 and signing the token with the same public key.

```python
import jwt # pip install pyjwt==1.5.0
PRIVATE_KEY = "-----BEGIN RSA PUBLIC KEY-----\nMIIBCgKCAQEAvoOtsfF5Gtkr2Swy0xzuUp5J3w8bJY5oF7TgDrkAhg1sFUEaCMlR\nYltE8jobFTyPo5cciBHD7huZVHLtRqdhkmPD4FSlKaaX2DfzqyiZaPhZZT62w7Hi\ngJlwG7M0xTUljQ6WBiIFW9By3amqYxyR2rOq8Y68ewN000VSFXy7FZjQ/CDA3wSl\nQ4KI40YEHBNeCl6QWXWxBb8AvHo4lkJ5zZyNje+uxq8St1WlZ8/5v55eavshcfD1\n0NSHaYIIilh9yic/xK4t20qvyZKe6Gpdw6vTyefw4+Hhp1gROwOrIa0X0alVepg9\nJddv6V/d/qjDRzpJIop9DSB8qcF1X23pkQIDAQAB\n-----END RSA PUBLIC KEY-----\n"
print(jwt.encode({'username': 'user', 'admin': True}, PRIVATE_KEY, algorithm='HS256').decode())
#eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6dHJ1ZX0.kQ9Cl3pOTU1ERqB4GZi3G-4bKkHzpnycu6giuKHC0uQ
```

### JSON in JSON

Create a session with the following username: <br>    user", "admin": "True <br>
This creates the body as {"admin": "False", "username": "user", "admin": "True"}.

### RSA or HMAC? Part 2

```python
# modified script from https://github.com/silentsignal/rsa_sign2n/blob/release/CVE-2017-11424/x_CVE-2017-11424.py

import sys
import jwt
import json
import base64
from gmpy2 import mpz,gcd,c_div
import binascii
from Crypto.Hash import SHA256
from Crypto.Signature import PKCS1_v1_5 # god bless http://ratmirkarabut.com/articles/ctf-writeup-google-ctf-quals-2017-rsa-ctf-challenge/
import asn1tools
import binascii
import time
import hmac
import hashlib

# I created 2 tokens, with username user1 and user2
jwt0="eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIxIiwiYWRtaW4iOmZhbHNlfQ.UWQ8HHOHtX5ADVCbIiLGilw7VwgkzvHKDi2h2H2rVCsG7nJCWqKRy5XdlgGFEK_92bG-jZDPep4NKz-pf2UlwzseZQAhdgxYz410Rl8Y0oGPv3kdKzQPZRcrSs8Jodg00fojEM88Tmiz5OeWayhi_ROgDS9Oai6gCQ5BOb2w0BpbjAyN3w4n4WXhsNtqhHjFyCM9JJBSECeFhjwHBNY1RaufkkfBlWo1gISRxVUlZJ85ji-rGWSLM4RBzhOBVu2aDS7ZfgYllx8HCegXUsmLqImPWwOfh5_n3K5VCrJN7MkQ4cH3kl--TciTYdgt5PsNtwwKZHd6q0M1-LShmvWIMA"
jwt1="eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIyIiwiYWRtaW4iOmZhbHNlfQ.vghJTYAv-C9u4ayDZHIyF8H0nwXnN2LFKaah3_3JJhW9iB-3cC2R1jkRBsiQ0RMLW6TqDOTqz7Yy44NAkX3Nlg2jK8XwWGhbKtEICYBZFxvUZaEuW2u33fEKirGk6she2UAX5gXaPicr9dOtZ7v8t_9yxGfKyr69budtVjThNSe9BpNvgAsLm-nirW7dIDdNyUI6t28WyXak-Z2_ZAr6iwBS6F8kwVXEwDQlrqIdKlWVES9iCn4YI6Tuzy1WAUcG-v8oX_Ze_yNWc4huEfj7eIkUUIwnY1X4fpbrUVvE6uThJxNRl5OYccByf1cnh8kaBUNyn6lwvGjHgKrnOzLGOw"

def b64urldecode(b64):
    return base64.urlsafe_b64decode(b64+("="*(len(b64) % 4)))
def b64urlencode(m):
    return base64.urlsafe_b64encode(m).strip(b"=")
def bytes2mpz(b):
    return mpz(int(binascii.hexlify(b),16))
def der2pem(der, token="RSA PUBLIC KEY"):
    der_b64=base64.b64encode(der).decode('ascii')
    lines=[ der_b64[i:i+64] for i in range(0, len(der_b64), 64) ]
    return "-----BEGIN %s-----\n%s\n-----END %s-----\n" % (token, "\n".join(lines), token)
def forge_mac(jwt0, public_key):
    jwt0_parts=jwt0.encode('utf8').split(b'.')
    jwt0_msg=b'.'.join(jwt0_parts[0:2])
    alg=b64urldecode(jwt0_parts[0].decode('utf8'))
    alg_tampered=b64urlencode(alg.replace(b"RS256",b"HS256"))

    # here i edit the payload as  {"username":"user1","admin":true}
    payload=json.loads( '{"username":"user1","admin":true}' )

    payload['exp'] = int(time.time())+86400
    payload_encoded=b64urlencode(json.dumps(payload).encode('utf8'))
    tamper_hmac=b64urlencode(hmac.HMAC(public_key,b'.'.join([alg_tampered, payload_encoded]),hashlib.sha256).digest())
    jwt0_tampered=b'.'.join([alg_tampered, payload_encoded, tamper_hmac])
    print("[+] Tampered JWT: %s" % (jwt0_tampered))

jwt0_sig_bytes = b64urldecode(jwt0.split('.')[2])
jwt1_sig_bytes = b64urldecode(jwt1.split('.')[2])
jwt0_sig = bytes2mpz(jwt0_sig_bytes)
jwt1_sig = bytes2mpz(jwt1_sig_bytes)
jks0_input = ".".join(jwt0.split('.')[0:2])
sha256_0=SHA256.new(jks0_input.encode('ascii'))
padded0 = PKCS1_v1_5.EMSA_PKCS1_V1_5_ENCODE(sha256_0, len(jwt0_sig_bytes))
jks1_input = ".".join(jwt1.split('.')[0:2])
sha256_1=SHA256.new(jks1_input.encode('ascii'))
padded1 = PKCS1_v1_5.EMSA_PKCS1_V1_5_ENCODE(sha256_1, len(jwt0_sig_bytes))
m0 = bytes2mpz(padded0) 
m1 = bytes2mpz(padded1)
pkcs1 = asn1tools.compile_files('pkcs1.asn', codec='der')
x509 = asn1tools.compile_files('x509.asn', codec='der')
for e in [mpz(3),mpz(65537)]:
    gcd_res = gcd(pow(jwt0_sig, e)-m0,pow(jwt1_sig, e)-m1)
    for my_gcd in range(1,100):
        my_n=c_div(gcd_res, mpz(my_gcd))
        if pow(jwt0_sig, e, my_n) == m0:
            pkcs1_pubkey=pkcs1.encode("RSAPublicKey", {"modulus": int(my_n), "publicExponent": int(e)})
            x509_der=x509.encode("PublicKeyInfo",{"publicKeyAlgorithm":{"algorithm":"1.2.840.113549.1.1.1","parameters":None},"publicKey":(pkcs1_pubkey, len(pkcs1_pubkey)*8)})
            pem_name = "%s_%d_x509.pem" % (hex(my_n)[2:18], e)
            with open(pem_name, "wb") as pem_out:
                public_key=der2pem(x509_der, token="PUBLIC KEY").encode('ascii')
                pem_out.write(public_key)
                forge_mac(jwt0, public_key)
            pem_name = "%s_%d_pkcs1.pem" % (hex(my_n)[2:18], e)
            with open(pem_name, "wb") as pem_out:
                public_key=der2pem(pkcs1_pubkey).encode('ascii')
                pem_out.write(public_key)
                forge_mac(jwt0, public_key)
    
#tampered JWT: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6ICJ1c2VyMSIsICJhZG1pbiI6IHRydWUsICJleHAiOiAxNjQwNTc1MTI2fQ.eYTJvTz4tWny9dPZDtwPjCu92FLBoTxYiQP8YpgFtYI
```
