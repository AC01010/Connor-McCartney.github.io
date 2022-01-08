---
title: ELLIPTIC CURVES - Side Channels
categories:
- CryptoHack
excerpt: |
  
---

This is my writeup of the [CryptoHack Elliptic Curves challenges](https://cryptohack.org/challenges/ecc/).

### Montgomery's Ladder

```python
#given
g_x = 9
#substitute then take modular sqrt
g_y = 14781619447589544791020593568409986887264606134616475288964881837755586237401

E = EllipticCurve(GF(2**255-19), [0,486662,0,1,0])
G = E.point((g_x, g_y))
Q = G*0x1337c0decafe
print(Q.xy()[0])
#crypto{49231350462786016064336756977412654793383964726771892982507420921563002378152}
```
