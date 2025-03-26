# PcapPoisoning

How about some hide and seek heh? Download this [file](https://artifacts.picoctf.net/c/372/trace.pcap) and find the flag.

-----

Opening the `trace.pcap` in Wireshark shows it is a capture of FTP traffic.  Following each stream found a login attempt, which was later retransmitted containing the flag:

```
picoCTF{P64P_4N4L7S1S_SU55355FUL_0f2d7dc9}
```