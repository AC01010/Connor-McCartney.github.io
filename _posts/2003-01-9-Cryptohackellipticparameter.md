---
title: ELLIPTIC CURVES - Parameter Choice
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Elliptic Curves challenges](https://cryptohack.org/challenges/ecc/).

### Smooth Criminal

We can use the Pohlig-Hellman algorithm as the curve is smooth (hint given in title). Sage's discrete_log has this built in.

```python
p = 310717010502520989590157367261876774703
a = 2
b = 3
E = EllipticCurve(GF(p), [a,b])

G = E(179210853392303317793440285562762725654, 105268671499942631758568591033409611165)
# Bob's public key
QB = E(272640099140026426377756188075937988094, 51062462309521034358726608268084433317)
# Our public key
QA = E(280810182131414898730378982766101210916, 291506490768054478159835604632710368904)

#Attack to find Bob's private key
nB = G.discrete_log(QB)

#shared secret
print((QA * nB).xy()[0])
```
With this we can decrypt with the usual given decrypt script from other challenges. <br>
crypto{n07_4ll_curv3s_4r3_s4f3_curv3s}

### Exceptional Curves

The curve parameters are vulnerable to Smart's attack. 

```python
def SmartAttack(P,Q,p):
    E = P.curve()
    Eqp = EllipticCurve(Qp(p, 2), [ ZZ(t) + randint(0,p)*p for t in E.a_invariants() ])
    P_Qps = Eqp.lift_x(ZZ(P.xy()[0]), all=True)
    for P_Qp in P_Qps:
        if GF(p)(P_Qp.xy()[1]) == P.xy()[1]:
            break
    Q_Qps = Eqp.lift_x(ZZ(Q.xy()[0]), all=True)
    for Q_Qp in Q_Qps:
        if GF(p)(Q_Qp.xy()[1]) == Q.xy()[1]:
            break
    p_times_P = p*P_Qp
    p_times_Q = p*Q_Qp
    x_P,y_P = p_times_P.xy()
    x_Q,y_Q = p_times_Q.xy()
    phi_P = -(x_P/y_P)
    phi_Q = -(x_Q/y_Q)
    k = phi_Q/phi_P
    return ZZ(k)

p = 0xa15c4fb663a578d8b2496d3151a946119ee42695e18e13e90600192b1d0abdbb6f787f90c8d102ff88e284dd4526f5f6b6c980bf88f1d0490714b67e8a2a2b77 
a = 0x5E009506FCC7EFF573BC960D88638FE25E76A9B6C7CAEEA072A27DCD1FA46ABB15B7B6210CF90CABA982893EE2779669BAC06E267013486B22FF3E24ABAE2D42 
b = 0x2CE7D1CA4493B0977F088F6D30D9241F8048FDEA112CC385B793BCE953998CAAE680864A7D3AA437EA3FFD1441CA3FB352B0B710BB3F053E980E503BE9A7FECE 
E = EllipticCurve(GF(p), [a, b])

#Our public key
QA = E(4748198372895404866752111766626421927481971519483471383813044005699388317650395315193922226704604937454742608233124831870493636003725200307683939875286865, 2421873309002279841021791369884483308051497215798017509805302041102468310636822060707350789776065212606890489706597369526562336256272258544226688832663757)
#Bob's public key
QB = E(0x7f0489e4efe6905f039476db54f9b6eac654c780342169155344abc5ac90167adc6b8dabacec643cbe420abffe9760cbc3e8a2b508d24779461c19b20e242a38, 0xdd04134e747354e5b9618d8cb3f60e03a74a709d4956641b234daa8a65d43df34e18d00a59c070801178d198e8905ef670118c15b0906d3a00a662d3a2736bf)
#generator
G = E(3034712809375537908102988750113382444008758539448972750581525810900634243392172703684905257490982543775233630011707375189041302436945106395617312498769005, 4986645098582616415690074082237817624424333339074969364527548107042876175480894132576399611027847402879885574130125050842710052291870268101817275410204850)

#Smart's Attack
nA = SmartAttack(G, QA, p)

#shared secret
print((QB * nA).xy()[0])
```

With this we can decrypt with the usual given decrypt script from other challenges. <br>
crypto{H3ns3l_lift3d_my_fl4g!}
