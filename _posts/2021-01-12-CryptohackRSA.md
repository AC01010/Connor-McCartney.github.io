---
title: RSA Writeups
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack RSA challenges](https://cryptohack.org/challenges/rsa).


### RSA Starter 1

Find the solution to 101<sup>17</sup> mod 22663.


SOLUTION <br>
Use the python pow function - pow(base, exponent, modulus)
```
print(pow(101, 17, 22663))
```
This gives 19906.

### RSA Starter 2

"Encrypt" the number 12 using the exponent e = 65537 and the primes p = 17 and q = 23. What number do you get as the ciphertext?


SOLUTION

We need to calculate the ciphertext number, let's call that c. 

We are given the number 12 to encrypt, usually this is part of a message so let's call that m. 

N = p x q = 17 * 23

In RSA, c = m<sup>e</sup> mod N.
<span style="color:blue">some *This is Blue italic.* text
We can calculate this in python:
```

m = 12
N = 17*23
e = 65537

c = pow(m, e, N)
print(c)

```
This gives c = 301.
</span>
### RSA Starter 3
Given p = 857504083339712752489993810777 and q = 1029224947942998075080348647219, what is the Euler totient of N?

SOLUTION

bleh
