# Silicon Data Sleuthing

Difficulty: Easy

In the dust and sand surrounding the vault, you unearth a rusty PCB... You try to read the etched print, it says Open..W...RT, a router! You hand it over to the hardware gurus and to their surprise the ROM Chip is intact! They manage to read the data off the tarnished silicon and they give you back a firmware image. It's now your job to examine the firmware and maybe recover some useful information that will be important for unlocking and bypassing some of the vault's countermeasures!

-----

```
$ cat etc/os-release 
NAME="OpenWrt"
VERSION="23.05.0"
ID="openwrt"
ID_LIKE="lede openwrt"
PRETTY_NAME="OpenWrt 23.05.0"
VERSION_ID="23.05.0"
HOME_URL="https://openwrt.org/"
BUG_URL="https://bugs.openwrt.org/"
SUPPORT_URL="https://forum.openwrt.org/"
BUILD_ID="r23497-6637af95aa"
OPENWRT_BOARD="ramips/mt7621"
OPENWRT_ARCH="mipsel_24kc"
OPENWRT_TAINTS=""
OPENWRT_DEVICE_MANUFACTURER="OpenWrt"
OPENWRT_DEVICE_MANUFACTURER_URL="https://openwrt.org/"
OPENWRT_DEVICE_PRODUCT="Generic"
OPENWRT_DEVICE_REVISION="v0"
OPENWRT_RELEASE="OpenWrt 23.05.0 r23497-6637af95aa"
```

https://openwrt.org/releases/23.05/notes-23.05.0

```
$ binwalk chal_router_dump.bin              

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
97696         0x17DA0         U-Boot version string, "U-Boot 1.1.3 (Aug 18 2020 - 11:10:29)"
98248         0x17FC8         CRC32 polynomial table, little endian
99600         0x18510         AES S-Box
100380        0x1881C         AES Inverse S-Box
458752        0x70000         gzip compressed data, maximum compression, from Unix, last modified: 2021-09-17 15:32:23
1572864       0x180000        uImage header, header size: 64 bytes, header CRC: 0x95E11ADB, created: 2023-10-09 21:45:35, image size: 2802312 bytes, Data Address: 0x80001000, Entry Point: 0x80001000, data CRC: 0x8055BE8E, OS: Linux, CPU: MIPS, image type: OS Kernel Image, compression type: none, image name: "MIPS OpenWrt Linux-5.15.134"
1578428       0x1815BC        Copyright string: "Copyright (C) 2011 Gabor Juhos <juhosg@openwrt.org>"
1578636       0x18168C        LZMA compressed data, properties: 0x6D, dictionary size: 8388608 bytes, uncompressed size: 9229911 bytes
4375240       0x42C2C8        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 3687796 bytes, 1328 inodes, blocksize: 262144 bytes, created: 2023-10-09 21:45:35
8126464       0x7C0000        JFFS2 filesystem, little endian
```

root:$1$YfuRJudo$cXCiIJXn9fWLIt8WY2Okp1:19804:0:99999:7:::

yohZ5ah

ae-h+i$i^Ngohroorie!bieng6kee7oh

VLT-AP01

french-halves-vehicular-favorable

1778,2289,8088

HTB{Y0u'v3_m4st3r3d_0p3nWRT_d4t4_3xtr4ct10n!!_b796aa38c791e345888a14c8febf20c5}