---
title: Cyber Santa is Coming to Town CTF
categories:
- Hack The Box
excerpt: |
  
---



### Common Mistake

Given:
n1 = 0xa96e6f96f6aedd5f9f6a169229f11b6fab589bf6361c5268f8217b7fad96708cfbee7857573ac606d7569b44b02afcfcfdd93c21838af933366de22a6116a2a3dee1c0015457c4935991d97014804d3d3e0d2be03ad42f675f20f41ea2afbb70c0e2a79b49789131c2f28fe8214b4506db353a9a8093dc7779ec847c2bea690e653d388e2faff459e24738cd3659d9ede795e0d1f8821fd5b49224cb47ae66f9ae3c58fa66db5ea9f73d7b741939048a242e91224f98daf0641e8a8ff19b58fb8c49b1a5abb059f44249dfd611515115a144cc7c2ca29357af46a9dc1800ae9330778ff1b7a8e45321147453cf17ef3a2111ad33bfeba2b62a047fa6a7af0eef <br>
e1 = 0x10001 <br>
c1 = 0x55cfe232610aa54dffcfb346117f0a38c77a33a2c67addf7a0368c93ec5c3e1baec9d3fe35a123960edc2cbdc238f332507b044d5dee1110f49311efc55a2efd3cf041bfb27130c2266e8dc61e5b99f275665823f584bc6139be4c153cdcf153bf4247fb3f57283a53e8733f982d790a74e99a5b10429012bc865296f0d4f408f65ee02cf41879543460ffc79e84615cc2515ce9ba20fe5992b427e0bbec6681911a9e6c6bbc3ca36c9eb8923ef333fb7e02e82c7bfb65b80710d78372a55432a1442d75cad5b562209bed4f85245f0157a09ce10718bbcef2b294dffb3f00a5a804ed7ba4fb680eea86e366e4f0b0a6d804e61a3b9d57afb92ecb147a769874<br>

n2 = 0xa96e6f96f6aedd5f9f6a169229f11b6fab589bf6361c5268f8217b7fad96708cfbee7857573ac606d7569b44b02afcfcfdd93c21838af933366de22a6116a2a3dee1c0015457c4935991d97014804d3d3e0d2be03ad42f675f20f41ea2afbb70c0e2a79b49789131c2f28fe8214b4506db353a9a8093dc7779ec847c2bea690e653d388e2faff459e24738cd3659d9ede795e0d1f8821fd5b49224cb47ae66f9ae3c58fa66db5ea9f73d7b741939048a242e91224f98daf0641e8a8ff19b58fb8c49b1a5abb059f44249dfd611515115a144cc7c2ca29357af46a9dc1800ae9330778ff1b7a8e45321147453cf17ef3a2111ad33bfeba2b62a047fa6a7af0eef<br>
e2 = 0x23<br>
c2 = 0x79834ce329453d3c4af06789e9dd654e43c16a85d8ba0dfa443aefe1ab4912a12a43b44f58f0b617662a459915e0c92a2429868a6b1d7aaaba500254c7eceba0a2df7144863f1889fab44122c9f355b74e3f357d17f0e693f261c0b9cefd07ca3d1b36563a8a8c985e211f9954ce07d4f75db40ce96feb6c91211a9ff9c0a21cad6c5090acf48bfd88042ad3c243850ad3afd6c33dd343c793c0fa2f98b4eabea399409c1966013a884368fc92310ebcb3be81d3702b936e7e883eeb94c2ebb0f9e5e6d3978c1f1f9c5a10e23a9d3252daac87f9bb748c961d3d361cc7dacb9da38ab8f2a1595d7a2eba5dce5abee659ad91a15b553d6e32d8118d1123859208 <br>


SOLUTION

n1 = n2 so we can use common modulus attack. 
```python
from Crypto.Util.number import long_to_bytes #pip install pycryptodome
from math import gcd

n = 0xa96e6f96f6aedd5f9f6a169229f11b6fab589bf6...........
e1 = 0x10001
e2 = 0x23
c1 = 0x55cfe232610aa54dffcfb346117f0a38c77a33a...........
c2 = 0x79834ce329453d3c4af06789e9dd654e43c16a5...........


def attack(c1, c2, e1, e2, n):
    s1 = pow(e1, -1, e2)
    s2 = int((gcd(e1,e2) - e1 * s1) / e2)
    temp = pow(c2, -1, n)
    m1 = pow(c1,s1,n)
    m2 = pow(temp,-s2,n)

    return (m1 * m2) % n

message = attack(c1, c2, e1, e2, n)
print(long_to_bytes(message).decode())
```
This gives HTB{c0mm0n_m0d_4774ck_15_4n07h3r_cl4ss1c}

### Toy Management
This was a web challenge with a simple login portal. 

SOLUTION

To login as admin I used the SQL injection admin'-- - as the username. <br>
This gave the flag HTB{1nj3cti0n_1s_in3v1t4bl3}

### XMAS Spirit

Now that elves have taken over Santa has lost so many letters from kids all over the world. However, there is one kid who managed to locate Santa and sent him a letter. It seems like the XMAS spirit is so strong within this kid. He was so smart that thought of encrypting the letter in case elves captured it. Unfortunately, Santa has no idea about cryptography. Can you help him read the letter?
<br>
I uploaded the files for this challenge at [https://github.com/Connor-McCartney/CTF-files/tree/main/CTF-XMAS-Spirit-files](https://github.com/Connor-McCartney/CTF-files/tree/main/CTF-XMAS-Spirit-files).

SOLUTION

In the file used to encrypt the pdf, we see enc = (a * byte + b) % mod. <br>
I'll rename enc to e (encrypted) and byte to l (the original letter). It's also given that mod = 256. <br>
Now it's time to do some math (modular arithmetic). We have e = (a * l + b) mod 256, and need to solve for l. <br>

First I subtracted b from both sides (because if a = b mod n, then a+x = b+x mod n) <br>
Next I swapped e-b and a * l (because if a = b mod n, then b = a mod n)    <br>
Next I multiplied both sides by a<sup>-1</sup> (the modular inverse of a) <br>
Now it makes sense why the encryption first checked that gcd(a, mod) == 1 when generating a random number for a, because this ensures that a has a modular inverse. 

e - b = (a * l) mod 256 <br>

l * a = (e - b) mod 256 <br>                 
l * a * a<sup>-1</sup> = ((e - b) * a<sup>-1</sup>) mod 256 <br> 

l = ((e - b) * a<sup>-1</sup>) mod 256      <br>

Now we can solve for l, except we don't know a or b so we'll have to bruteforce. My first idea was to decrypt the entire message, but I soon realised that would take too much computing and wasn't feasible. Then I decided to analyse another pdf I had on my computer. <br>
I ran "xxd random-file.pdf | head" and noticed the first 4 characters of a pdf file are "%PDF". Using this I wrote a scipt to bruteforce a and b. <br>
```python
from math import gcd

possible_b = [i for i in range(1, 256)]
possible_a = []
for i in range(256):
    if(gcd(256, i) == 1):
        possible_a.append(i)

for a in possible_a:
    for b in possible_b:
            with open("encrypted.bin", "rb") as f:
                letter = ""
                for i in range(4):
                    e = int.from_bytes(f.read(1), 'big')
                    l = ((e - b) * pow(a, -1, 256)) % 256
                    letter += chr(l)

                if "%PDF" in letter:
                    print(f"a = {a} ")
                    print(f"b = {b}")
```

This gives <br>
a = 169 <br>
b = 160 <br>

Now we know a and b we can decode the entire pdf. 

```python
a =  169
b =  160

letter = b''
with open("encrypted.bin", "rb") as f:
    while (e := f.read(1)):
        e = int.from_bytes(e, 'big')
        l = ((e-b) * pow(a, -1, 256)) % 256
        letter += bytes([l])
f.close()

with open('letter.pdf', 'wb') as g:
    g.write(letter)
g.close()
```
This creates the pdf, and when you open it you see the flag.

### Missing Reindeer
We are given: <br>
ciphertext = ""Ci95oTkIL85VWrJLVhns1O2vyBeCd0weKp9o3dSY7hQl7CyiIB/ <br>
D3HaXQ619k0+4FxkVEksPL6j3wLp8HMJAPxeA321RZexR9qwswQv2S6xQ3QFJi6sgv <br>
xkN0YnXtLKRYHQ3te1Nzo53gDnbvuR6zWV8fdlOcBoHtKXlVlsqODku2GvkTQ/06x8 <br>
zOAWgQCKj78V2mkPiSSXf2/qfDp+FEalbOJlILsZMe3NdgjvohpJHN3O5hLfBPdod2 <br>
v6iSeNxl7eVcpNtwjkhjzUx35SScJDzKuvAv+6DupMrVSLUfcWyvYUyd/l4v01w+8wvPH9l"

and the public key: <br>

-----BEGIN PUBLIC KEY----- <br>
MIIBIDANBgkqhkiG9w0BAQEFAAOCAQ0AMIIBCAKCAQEA5iOXKISx9NcivdXuW+uE<br>
y4R2DC7Q/6/ZPNYDD7INeTCQO9FzHcdMlUojB1MD39cbiFzWbphb91ntF6mF9+fY<br>
N8hXvTGhR9dNomFJKFj6X8+4kjCHjvT//P+S/CkpiTJkVK+1G7erJT/v1bNXv4Om<br>
OfFTIEr8Vijz4CAixpSdwjyxnS/WObbVmHrDMqAd0jtDemd3u5Z/gOUi6UHl+XIW<br>
Cu1Vbbc5ORmAZCKuGn3JsZmW/beykUFHLWgD3/QqcT21esB4/KSNGmhhQj3joS7Z<br>
z6+4MeXWm5LXGWPQIyKMJhLqM0plLEYSH1BdG1pVEiTGn8gjnP4Qk95oCV9xUxWW<br>
ZwIBAw==<br>
-----END PUBLIC KEY-----


SOLUTION
  
  
We can extract n and e from the public key. n is too large to be factorised, but e=3. <br>
This might be vulnerable to a small exponent attack. <br>
This occurs when m<sup>e</sup> is less than n (m = message,e = exponent, n = modulus). <br>
After a quick check, it is vulnerable. To decrypt using this attack, m = the e<sup>th</sup> root of the ciphertext.

```python
import base64
from Crypto.Util.number import bytes_to_long, long_to_bytes #pip install pycryptodome

c = bytes_to_long(base64.b64decode(b"""Ci95oTkIL85VWrJLVhns1O2vyBeCd0weKp9o3dSY7hQl7CyiIB/D3HaXQ619k0+
4FxkVEksPL6j3wLp8HMJAPxeA321RZexR9qwswQv2S6xQ3QFJi6sgvxkN0YnXtLKRYHQ3te1Nzo53gDnbvuR6zWV8fdlOcBoHtKXlV
lsqODku2GvkTQ/06x8zOAWgQCKj78V2mkPiSSXf2/qfDp+FEalbOJlILsZMe3NdgjvohpJHN3O5hLfBPdod2v6iSeNxl7eVcpNtwj
khjzUx35SScJDzKuvAv+6DupMrVSLUfcWyvYUyd/l4v01w+8wvPH9l"""))

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

print(long_to_bytes(nth_root(c, 3)).decode())
```
  
This gives 'We are in Antarctica, near the independence mountains.HTB{w34k_3xp0n3n7_ffc896}'
