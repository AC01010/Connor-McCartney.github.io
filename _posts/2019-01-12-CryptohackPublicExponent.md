---
title: RSA Public Exponent
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack RSA challenges](https://cryptohack.org/challenges/rsa).


### Salty

This challenge is vulnerable to a small exponent attack. This occurs when m<sup>e</sup> is less than n. To decrypt using this attack, m = the eth root of the ciphertext.
Here e=1, so m is simply c. 
```python
from Crypto.Util.number import bytes_to_long, long_to_bytes #pip install pycryptodome

c = 44981230718212183604274785925793145442655465025264554046028251311164494127485
print(long_to_bytes(c).decode())
```
This gives crypto{saltstack_fell_for_this!}

### Modulus Inutilis

We can use small exponent attack again. 
```python
from Crypto.Util.number import long_to_bytes #pip install pycryptodome

c = 243251053617903760309941844835411292373350655973075480264001352919865180151222189820473358411037759381328642957324889519192337152355302808400638052620580409813222660643570085177957

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
This gives crypto{N33d_m04R_p4dd1ng}

### Everything is Big

For large e, d *might* be small, allowing [Wiener's attack](https://en.wikipedia.org/wiki/Wiener%27s_attack).
```python
import owiener #pip3 install owiener
from Crypto.Util.number import long_to_bytes

n = 0x8da7d2ec7bf9b322a539afb9962d4d2ebeb3e3d449d709b80a51dc680a14c87ffa863edfc7b5a2a542a0fa610febe2d967b58ae714c46a6eccb44cd5c90d1cf5e271224aa3367e5a13305f2744e2e56059b17bf520c95d521d34fdad3b0c12e7821a3169aa900c711e6923ca1a26c71fc5ac8a9ff8c878164e2434c724b68b508a030f86211c1307b6f90c0cd489a27fdc5e6190f6193447e0441a49edde165cf6074994ea260a21ea1fc7e2dfb038df437f02b9ddb7b5244a9620c8eca858865e83bab3413135e76a54ee718f4e431c29d3cb6e353a75d74f831bed2cc7bdce553f25b617b3bdd9ef901e249e43545c91b0cd8798b27804d61926e317a2b745
e = 0x86d357db4e1b60a2e9f9f25e2db15204c820b6e8d8d04d29db168c890bc8a6c1e31b9316c9680174e128515a00256b775a1a8ccca9c6936f1b4c2298c03032cda4dd8eca1145828d31466bf56bfcf0c6a8b4a1b2fb27de7a57fae7430048d7590734b2f05b6443ad60d89606802409d2fa4c6767ad42bffae01a8ef1364418362e133fa7b2770af64a68ad50ad8d2bd5cebb99ceb13368fb31a6e7503e753f8638e21a96af1b6498c18578ba89b98d70fa482ad137d28fe701b4b77baa25d5e84c81b26ee9bddf8cbb51a071c60dd57714de379cd4bc14932809ba18524a0a18e4133665cfc46e2c4fcfbc28e0a0957e5513a7307c422b87a6182d0b6a074b4d
c = 0x6a2f2e401a54eeb5dab1e6d5d80e92a6ca189049e22844c825012b8f0578f95b269b19644c7c8af3d544840d380ed75fdf86844aa8976622fa0501eaec0e5a1a5ab09d3d1037e55501c4e270060470c9f4019ced6c4e67673843daf2fd71c64f3dd8939ae322f2b79d283b3382052d076ebe9bb50b0042f1f7dd7beadf0f5686926ade9fc8370283ead781a21896e7a878d99e77c3bb1f470401062c0e0327fd85da1cf12901635f1df310e8f8c7d87aff5a01dbbecd739cd8f36462060d0eb237af8d613e2d9cebb67d612bcfc353ef2cd44b7ac85e471287eb04ae9b388b66ea8eb32429ae96dba5da8206894fa8c58a7440a127fceb5717a2eaa3c29f25f7
d = owiener.attack(e, n)
m = pow(c, d, n)
print(long_to_bytes(m).decode())
```
This gives crypto{s0m3th1ng5_c4n_b3_t00_b1g}.

### Crossed Wires

This challenge involved a lot of modular arithmetic. <br>
We are given the ciphertext c, exponent e = 65537, n and d from our private key, and exponents e<sub>1</sub>, e<sub>2</sub>, e<sub>3</sub>, e<sub>4</sub>, e<sub>5</sub> from the friend's public keys. The modulus n is the same for all private and public keys. <br>
The message m is encrypted with all of the friend's public keys: <br>
c = ((((m<sup>e1</sup> mod n)<sup>e2</sup> mod n)<sup>e3</sup> mod n)<sup>e4</sup> mod n)<sup>e5</sup> mod n 

Each exponent from the friend's public key must be decrypted with a corresponding d. <br>
m = ((((c<sup>d5</sup> mod n)<sup>d4</sup> mod n)<sup>d3</sup> mod n)<sup>d2</sup> mod n)<sup>d1</sup> mod n 

Since (x<sup>a</sup> mod n)<sup>b</sup> mod n = x<sup>ab</sup> mod n, this simplifies to:  <br>
m = c<sup>d1 x d2 x d3 x d4 x d5</sup> mod n

Next we must solve for d1 x d2 x d3 x d4 x d5. Note I'll refer to phi(n) as just phi. <br>
e1 * d1 = 1 mod phi <br>

So long as k < e1, we can use the fact that: <br>
e1(e1<sup>-1</sup> mod (k * phi)) = 1 mod phi

Thus, <br>
d1 = e1<sup>-1</sup> mod (k * phi)
                           
We know in RSA that ed = 1 mod phi. From this we can get k x phi = ed - 1. Now we substitute this:<br>
d1 = e1<sup>-1</sup> mod (ed - 1)

We can repeat this for d2, d3, d4 and d5.

d1 x d2 x d3 x d4 x d5 = (e1<sup>-1</sup> mod (ed - 1))(e2<sup>-1</sup> mod (ed - 1))(e3<sup>-1</sup> mod (ed - 1))(e4<sup>-1</sup> mod (ed - 1))(e5<sup>-1</sup> mod (ed - 1))<br>d1 x d2 x d3 x d4 x d5 = (e1 x e2 x e3 x e4 x e5)<sup>-1</sup> mod (ed - 1)
       
If we sub this in we get:
m = c<sup>(e1 x e2 x e3 x e4 x e5)<sup>-1</sup> mod (ed - 1)</sup> mod n
       
We can calculate this in python:
```python
from Crypto.Util.number import long_to_bytes, inverse

e = 65537
c = int("""20304610279578186738172766224224793119885071262464464448863461184092225736054747976985179673905441502689126216282897704508745403799054734121583968853999791604281615154100
7362591314534243853643246302296711853437781728072626407093018382748246031016924856627262269021211055911374373314632018812642455622140121608751771674420109524393606233966589744139004
6909383679475227039952007459632905872587483408218869737759794940577903913919419606536442621320834546140703077108978752920005710574658449355472279059253047286958131011730034346120775
0821737840042745530876391793484035024644475535353227851321505537398888106855012746117""".replace("\n", ""))
n = int("""21711308225346315542706844618441565741046498277716979943478360598053144971379956916575370343448988601905854572029635846626259487297950305231661109855854947494209135205589
2586435179615215949243684986720642932082308024410773901936829580951119220826778131758047756288843777243776474283858418312770592741729822805452377655599692287075068575612152684910240
9706392033772178367306053018163716157740158912655855618254689678330737051727504652270404738578611148944706479421001080276170861590724552349258589628637499608808931782616279827852829
6206977900274431829829206103227171839270887476436899494428371323874689055690729986771""".replace("\n", ""))
d = int("""27344116772511480307231380057161097338388665453755276020182551593196310266531907836704931079364016039814291718805043605604947710172464687029026473709542203124525413428587
4759057627377510787045085353371711668432697626300643573338204580797189076201874772957402105743033177803398235918483815974733123653850184996532926477492760757041034701941840745193787
5684373454982306923178403161216817237890962651214718831954215200637651103907209347900857824722653217179548148145687181377220544864521808230122730967452981435355334932104265488075777
638608041325256776275200067541533022527964743478554948792578057708522350812154888097""".replace("\n", ""))

m = pow(c, pow(106979 * 108533 * 69557 * 97117 * 103231, -1, e*d-1) , n)
print(long_to_bytes(m).decode())
```

This gives crypto{3ncrypt_y0ur_s3cr3t_w1th_y0ur_fr1end5_publ1c_k3y}.


