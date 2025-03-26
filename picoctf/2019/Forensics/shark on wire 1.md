# shark on wire 1

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/483e50268fe7e015c49caf51a69063d0/capture.pcap). Recover the flag.

-----

Opening the packet file within Wireshark shows a large number of UDP traffic. Following through each stream shows communication between `10.0.0.2` and `10.0.0.12` which sends the flag one character (byte) at a time:

```
picoCTF{StaT31355_636f6e6e}
```