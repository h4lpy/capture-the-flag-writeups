# Eavesdrop

Download this packet capture and find the flag.

- [Download packet capture](https://artifacts.picoctf.net/c/134/capture.flag.pcap)

-----

We are given a PCAP, `capture.flag.pcap`, which contains the following stream:

![[picoctf/images/Pasted image 20250225142714.png]]

```
openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
```

This file has been shared (packet 57) which corresponds to:

```
53616c7465645f5f0c160de825c4925b6543fa6c52dc91b4487b8252966d9de6aec8508b95522f879232bf89e3066440
```

Using **File -> Export Packet Bytes**, we can decode the file to retrieve the flag:

```
$ openssl des3 -d -salt -in file.dat -out file.txt -k supersecretpassword123
$ cat file.txt
picoCTF{nc_73115_411_dd54ab67}
```

```
picoCTF{nc_73115_411_dd54ab67}
```