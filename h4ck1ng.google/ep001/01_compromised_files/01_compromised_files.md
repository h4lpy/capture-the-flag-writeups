# Challenge 01 | Compromised Files

## Overview

Your files have been compromised, get them back.

HInt: Find a way to make sense of it.



## Walkthrough



```
for f in *.pem; do (./wannacry -encrypted_file flag -key_file $f); done | strings | grep solve  
https://h4ck1ng.google/solve/CrY_n0_m0r3
```


![[Pasted image 20221010161505.png]]

![[Pasted image 20221010161645.png]]

Flag: `https://h4ck1ng.google/solve/1_w0nd3r_wh47_53cr3t5_l13_h3r3`

