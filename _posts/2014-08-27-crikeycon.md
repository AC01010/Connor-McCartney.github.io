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

The password hash format for /etc/shadow file fields is $id$salt$hashed. <br>
So id is 6, <br>
salt is KbmN9bCSWQK2.h5o, <br>
and hash is ExR7Lh4OeLBP3173vBPWmgp/QHlaQu3U/PcNURnHalelr/l34hw.2kifKIPNkUdhiLk/f7HTaKhS2DACSCXJz/

https://hashcat.net/wiki/doku.php?id=example_hashes <br>
id is 6 means it is a SHA-512 hash. We can pass the whole hash into hashcat and we find the password! "starwars":

{% include figure.html image="https://i.postimg.cc/dQ4mp6H7/hashcat1.png" position="left" width="1206" height="526" %}

{% include figure.html image="https://i.postimg.cc/Yq0fHFZd/hashcat2.png" position="left" width="1064" height="733" %}

