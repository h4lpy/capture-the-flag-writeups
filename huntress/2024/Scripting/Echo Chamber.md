# Echo Chamber

Is anyone there? Is anyone there? I'm sending myself the flag! I'm sending myself the flag!

File: [echo_chamber.pcap]()

-----

Opening the provided PCAP, we see it contains ICMP echo request and echo replies.

```
# Filter the PCAP for ICMP Echo packets and extract the data
$ tshark -r echo_chamber.pcap -Y 'icmp.type == 8' -T fields -e data.data
```



![[huntress/2024/images/image.png]]

```
flag{6b38aa917a754d8bf384dc73fde633ad}
```

