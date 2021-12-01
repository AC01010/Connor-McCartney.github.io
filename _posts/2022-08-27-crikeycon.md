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
The password format for /etc/shadow file fields is $id$salt$hashed. <br>
So id is 6, <br>
salt is KbmN9bCSWQK2.h5o, <br>
and hash is ExR7Lh4OeLBP3173vBPWmgp/QHlaQu3U/PcNURnHalelr/l34hw.2kifKIPNkUdhiLk/f7HTaKhS2DACSCXJz/

id is 6 means it is a SHA-512 hash. We can pass the whole thing into hashcat. First create a hash file. Then, 


