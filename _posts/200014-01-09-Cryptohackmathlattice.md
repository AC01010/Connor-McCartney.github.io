---
title: MATHEMATICS - Lattices
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Mathematics challenges](https://cryptohack.org/challenges/maths/).

### Vectors

Using sage:

```python
v, w, u = vector( (2,6,3) ), vector( (1,0,0) ), vector( (7,7,2) )
print((3*(2*v - w)).dot_product(2*u))
#702
```

### Size and Basis

Using sage:

```python
v = vector( (4,6,2,5) )
print(sqrt(v.dot_product(v)))
#9
```

### Gram Schmidt

Using sage:

```python
A = matrix([[4,1,3,-1], [2,1,-3,4], [1,0,-2,7], [6, 2, 9, -5]])
x = A.gram_schmidt()[0][3][1]
print(round(x, 5))
#0.91611
```

### What's a Lattice?

Using sage:

```python
A=matrix(3,3,[6, 2, -3, 5, 1, 4, 2, 7, 1])
print(abs(A.det()))
#255
```

### Gaussian Reduction

Using sage:

```python
def gauss_reduction(v1,v2):
    while true:
        if v2.norm() < v1.norm():
            tmp = v1
            v1 = v2
            v2 = tmp
        m = floor(v1.dot_product(v2) / v1.dot_product(v1))
        if m == 0:
            return v1,v2
        v2 = v2 - m * v1

v1,v2 = gauss_reduction(vector([846835985, 9834798552]),vector([87502093, 123094980]))
print(v1*v2)
#7410790865146821
```

### Backpack Cryptography

Using sage:

```python
with open('./output.txt', 'r') as f:
    H = list(map(int, f.readline().replace("Public key: [", "").replace("]\n", "").split(", "))) #public key
    c = int(f.readline().replace("Encrypted Flag: ", "")) #ciphertext

N = len(H)
M = 2 * identity_matrix(N)
M = M.insert_row(N, [1 for x in range(N)])
M = M.augment(matrix([[x] for x in H] + [[c]]))
"""
Constructs matrix M as: 
2 0 0 0 ... H_1
0 2 0 0 ... H_2
0 0 2 0 ... ...
0 0 0 2 ... H_N
1 1 1 1 ... c
"""

#we can recover plaintext from solution of LLL algorithm
B = M.LLL()
for v in B:
   S = 0
   for i in range(len(v)):
      if v[i] < 0:
         S += H[i]
   if S == c:
      break

flag = "".join(['1' if i==-1 else '0' for i in v[-2:0:-1]]) #binary string
print(''.join(chr(int(flag[i * 8: i * 8 + 8], 2)) for i in range(len(flag) //8)))
#crypto{my_kn4ps4ck_1s_l1ghtw31ght}
```


