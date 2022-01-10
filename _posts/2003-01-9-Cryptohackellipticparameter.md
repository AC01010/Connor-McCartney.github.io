---
title: ELLIPTIC CURVES - Parameter Choice
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Elliptic Curves challenges](https://cryptohack.org/challenges/ecc/).

### Smooth Criminal

We can use the Pohlig-Hellman algorithm as the curve is smooth (hint given in title).

crypto{n07_4ll_curv3s_4r3_s4f3_curv3s}

### Exceptional Curves

The curve parameters are vulnerable to Smart's attack. 

```python
def SmartAttack(P, Q, p):
    E = P.curve()
    Eqp = EllipticCurve(Qp(p, 2), [ZZ(t) + randint(0, p) * p for t in E.a_invariants()])

    P_Qps = Eqp.lift_x(ZZ(P.xy()[0]), all=True)
    for P_Qp in P_Qps:
        if GF(p)(P_Qp.xy()[1]) == P.xy()[1]:
            break

    Q_Qps = Eqp.lift_x(ZZ(Q.xy()[0]), all=True)
    for Q_Qp in Q_Qps:
        if GF(p)(Q_Qp.xy()[1]) == Q.xy()[1]:
            break

    p_times_P = p * P_Qp
    p_times_Q = p * Q_Qp

    x_P, y_P = p_times_P.xy()
    x_Q, y_Q = p_times_Q.xy()

    phi_P = -(x_P / y_P)
    phi_Q = -(x_Q / y_Q)
    k = phi_Q / phi_P
    return ZZ(k)


def dec(cipher, number):
    Inn = sha3_512(str(number).encode()).hexdigest()
    Inn = long_to_bytes(int(Inn, 16))
    key = Inn[:16]
    iv = Inn[32:48]
    aes = AES.new(key, AES.MODE_CBC, iv)
    m = aes.decrypt(bytes.fromhex(cipher))
    return m


p = 0xa15c4fb663a578d8b2496d3151a946119ee42695e18e13e90600192b1d0abdbb6f787f90c8d102ff88e284dd4526f5f6b6c980bf88f1d0490714b67e8a2a2b77 #changed
a = 0x5E009506FCC7EFF573BC960D88638FE25E76A9B6C7CAEEA072A27DCD1FA46ABB15B7B6210CF90CABA982893EE2779669BAC06E267013486B22FF3E24ABAE2D42 #changed
b = 0x2CE7D1CA4493B0977F088F6D30D9241F8048FDEA112CC385B793BCE953998CAAE680864A7D3AA437EA3FFD1441CA3FB352B0B710BB3F053E980E503BE9A7FECE #changed

E = EllipticCurve(GF(p), [a, b])
P = E(6688995459156889342833492746830178561351032451553431663459092539202079309331606721889305095673315425664498757903172117433693024371601071704276267727669952, 5564994349416751535150119074854383530374616150731361990032636138300946966131834924594759776879733710221037510456861691212827465644751045328004911650582627)
Q = E(0x7f0489e4efe6905f039476db54f9b6eac654c780342169155344abc5ac90167adc6b8dabacec643cbe420abffe9760cbc3e8a2b508d24779461c19b20e242a38, 0xdd04134e747354e5b9618d8cb3f60e03a74a709d4956641b234daa8a65d43df34e18d00a59c070801178d198e8905ef670118c15b0906d3a00a662d3a2736bf)


print(P.order() == p)
d = SmartAttack(Q, P, p)
print(d)
```

This isn't working, but once we find d, shared secret equals

```python
print((Q * d).xy()[0])
```

and we can decrypt with the decrypt script from other challenge. 
