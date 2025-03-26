# Keyboard Junkie

My friend wouldn't shut up about his new keyboard, so...

File: [keyboard_junkie]()

-----

**Keyboard Junkie** was a Forensics challenge released on Day #14 of the Huntress Labs Capture the Flag (CTF) competition. We were provided a PCAP file, `keyboard_junkie`, containing a the communication of a keyboard via USB and tasked with extracting the keys pressed.

![](https://miro.medium.com/v2/resize:fit:700/1*mbWvXUPFa5tJ7JN0xdlsuQ.png)

```
$ file keyboard_junkie.pcap  
keyboard_junkie.pcap: pcap capture file, microsecond ts (little-endian) - version 2.4 (Memory-mapped Linux USB, capture length 245824)
```

This is a fairly common CTF challenge, so there are good tools and documentation to help solve it. The encoded keystrokes correspond to the data shown in table 12 within the [USB HID Usage Tables](https://usb.org/sites/default/files/documents/hut1_12v2.pdf):

![](https://miro.medium.com/v2/resize:fit:700/1*Y8fH4nI1xV5XsVvsaHRPFg.png)

To solve this, I first used `tshark` to extract the keypresses to a file, `keystrokes.txt` and then used [ctf-usb-keyboard-parser by TeamRocketIst on GitHub](https://github.com/TeamRocketIst/ctf-usb-keyboard-parser) to decode the keypresses from hex:

```
# Extract keystrokes (encoded) from pcap
$ tshark -r ./keyboard_junkie.pcap -Y 'usb.capdata && sb.data_len == 8' -T fields -e usb.capdata | sed 's/../:&/g2' > keystrokes.txt

# decode with ctf-usb-keyboard-parser
$ git clone https://github.com/TeamRocketIst/ctf-usb-keyboard-parser
$ python3 ctf-usb-keybaord-parser/usbkeyboard.py ./keystrokes.txt
```

![](https://miro.medium.com/v2/resize:fit:700/1*Czebbq_pjNFS_4SpL3K86w.png)

```
flag{f7733e0093b7d281dd0a30fcf34a9634}
```

# References

HackTricks. _USB Keystrokes_. [https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/pcap-inspection/usb-keystrokes](https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/pcap-inspection/usb-keystrokes)

Universal Serial Bus (USB) Hid Usage Tables. [https://usb.org/sites/default/files/documents/hut1_12v2.pdf](https://usb.org/sites/default/files/documents/hut1_12v2.pdf)

TeamRocketIst — GitHub. _ctf-usb-keyboard-parser_. [https://github.com/TeamRocketIst/ctf-usb-keyboard-parser](https://github.com/TeamRocketIst/ctf-usb-keyboard-parser)