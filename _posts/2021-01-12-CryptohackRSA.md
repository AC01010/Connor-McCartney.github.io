---
title: RSA Writeups
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack RSA challenges](https://cryptohack.org/challenges/rsa).


### RSA Starter 1

QUESTION

Find the solution to 101<sup>17</sup> mod 22663.


SOLUTION

Use the python pow function - pow(base, exponent, modulus)
<br>
`print(pow(101, 17, 22663))`
<br>
This gives 19906.

### RSA Starter 2

QUESTION

"Encrypt" the number 12 using the exponent e = 65537 and the primes p = 17 and q = 23. What number do you get as the ciphertext?


SOLUTION
