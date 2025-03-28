# Advent of Cyber - Side Quest

## Overview

This room runs alongside [TryHackMe's Advent of Cyber 2023](https://tryhackme.com/room/adventofcyber2023) with keys located throughout the main event. 

## Quest 1

This key is divided into four parts. Three will be posted on our social media channels between Tuesday, 28th November and Thursday, 30th November. The final part of the key will be posted in this room, on December 1st, 4 pm GMT. Find them all, put them together, and uncover the link to the first secret challenge! All the links to our social media channels can be found in Task 3 of the main [Advent of Cyber](https://tryhackme.com/room/adventofcyber2023) room.

The first part of the QR code was posted on the [TryHackMe discord](https://discord.com/channels/521382216299839518/1176552309707264041/1179095411420577943) at `2023-11-28 16:24:00 UTC`:

![Advent of Cyber 2023 Side Quest 1 - QR 1](tryhackme/images/aoc2023sq1_discord.png)

The second part comes from a [Tweet](https://twitter.com/RealTryHackMe/status/1730184898365767880) by `@RealTryHackMe` on `2023-11-30 11:19:00 UTC` containing link to `hubs.la/Q02btlld0`:

![Advent of Cyber 2023 Side Quest 1 - Tweet](tryhackme/images/aoc2023sq1_tweet.png)

Redirects to [assets.tryhackme.com](https://assets.tryhackme.com/additional/aoc2023/2f7f8/0f93a.png):

```console
$ wget https://assets.tryhackme.com/additional/aoc2023/2f7f8/0f93a.png
```

[LinkedIn]() post by [TryHackMe]() with link to `` which redirects to [assets.tryhackme.com](https://assets.tryhackme.com/additional/aoc2023/5d60a/809cd.png)


Final piece posted in [Advent of Cyber Side Quest room](https://tryhackme.com/room/adventofcyber23sidequest):

```
https://assets.tryhackme.com/additional/aoc2023/6d156/50af2.png

https://assets.tryhackme.com/additional/aoc2023/b3620/e94fa.png

https://assets.tryhackme.com/additional/aoc2023/5d60a/809cd.png

https://assets.tryhackme.com/additional/aoc2023/2f7f8/0f93a.png

```

Final QR code:

![Advent of Cyber 2023 Side Quest 1 - Final QR Code](tryhackme/images/aoc2023sq1_final_qr.png)

Scanning this QR code redirects us to an unreleased TryHackMe room - [The Return of the Yeti](https://tryhackme.com/jr/adv3nt0fdbopsjcap). For this room, we are supplied a PCAPNG file `VanSpy.pcapng`.

Opening this in Wireshark shows this is a dump of Wifi traffic, with the SSID shown as `FreeWifiBFC`:

![Advent of Cyber 2023 Side Quest 1 - SSID](tryhackme/images/aoc2023sq1_ssid.png)

To obtain the password, we first need to convert the `pcapng` file to a format which Hashcat accepts. We can use `hcxpcapngtool` from [hcxtools](https://github.com/ZerBea/hcxtools):

```console
$ sudo hcxpcapngtool -o VanSpy.hash -E /usr/share/wordlists/rockyou.txt VanSpy.pcapng
```

We supply the `VanSpy.pcapng` file in addition to the `rockyou.txt` wordlist, outputting the result to `VanSpy.hash` which utilises the 22000 hash format.

Next, we can crack the password for the network with Hashcat:

```console
$ hashcat -m 22000 VanSpy.hash /usr/share/wordlists/rockyou.txt
$ hashcat -m 22000 VanSpy.hash /usr/share/wordlists/rockyou.txt --show
```

![Advent of Cyber 2023 Side Quest 1 - Wifi Password](tryhackme/images/aoc2023sq1_wifi_password.png)

![Advent of Cyber 2023 Side Quest 1 - Aircrack-ng](tryhackme/images/aoc2023sq1_aircrack.png)

## Quest 2

This key will be hidden in one of the challenges of the main Advent of Cyber event between Day 2 and Day 8. Look for clues to find out which challenge to dig into!

From Day 6, Memories of Christmas Past, we get an indication that the second side quest is hidden in this room:

```
Van Jolly still thinks the Ghost of Christmas Past is in the game. She says she has seen it with her own eyes! She thinks the Ghost is hiding in a glitch, whatever that means. What could she have seen?
```

For the initial set up, we can set our name to `AAAAAAAAAAAAOOPS` after getting 16 coins from the PC.

![Advent of Cyber 2023 Side Quest 2 - Setup](tryhackme/images/aoc2023sq2_setup.png)

Speaking to the shopkeeper, Van Holly, we see they have 12 items for sale, some of which are sold out:

![Advent of Cyber 2023 Side Quest 2 - Shop Items](tryhackme/images/aoc2023sq2_shop_items.png)

We know that we can give ourselves these items using the buffer overflow technique showcased in the room, but looking more closely at the item IDs, we see that `a` is missing. Attempting to select `a` will generate this error:

![Advent of Cyber 2023 Side Quest 2 - a error](tryhackme/images/aoc2023sq2_a_error.png)

Since we know we can control the content of our inventory, we can give ourselves the `a` item using the following payload:

```
AAAAAAAAAAAAOOPSAAAAAAAAAAAAAAAAAAAAAAAAAAAAa
```

![Advent of Cyber 2023 Side Quest 2 - a Item Overflow](tryhackme/images/aoc2023sq2_a_item_overflow.png)

From the above, we now have a yeti token. However, when we interact with the shopkeeper, he states that this is a fake, but notes he can sell us the real one for a fee:

![Adevent of Cyber 2023 Side Quest 2 - Fake Yeti Token](tryhackme/images/aoc2023sq2_fake_yeti_token.png)

As we saw previously, we don't have enough coins to simply buy `a` from the store, so we have to give ourselves more coins. From the [ASCII table](https://www.asciitable.com/), the character which will give us the largest number of coins on a standard keyboard is `~`, so changing our initial setup payload to the following will give us the maximum number of coins and allow us to buy the real yeti token:

```
AAAAAAAAAAAA~~~~
```

![Advent of Cyber 2023 Side Quest 2 - Real Yeti Token](tryhackme/images/aoc2023sq2_real_yeti_token.png)

As shown in the above screenshot, the mysterious Ghost of Christmas Past appears at the house, stating a **cat named Snowball** will arrive here one day where he will be greeted by **Midas the merchant** and **Ted the name switcher** and that he will bring **31337 coins** and the **yeti token**. When these conditions are met, a secret code is inputted and the hidden code is revealed:

![Advent of Cyber 2023 Side Quest 2 - GOCP Snowball](tryhackme/images/aoc2023sq2_gocp_snowball.png)

![Advent of Cyber 2023 Side Quest 2 - GOCP Midas and Ted](tryhackme/images/aoc2023sq2_gocp_midas_ted.png)

![Advent of Cyber 2023 Side Quest 2 - GOCP Coin](tryhackme/images/aoc2023sq2_gocp_coin.png)

![Advent of Cyber 2023 Side Quest 2 - GOCP Secret Code](tryhackme/images/aoc2023sq2_gocp_secret_code.png)



![](Pasted%20image%2020231219140946.png)

30 lives / konami code:

https://en.wikipedia.org/wiki/Konami_Code

![](Pasted%20image%2020231219141941.png)

![](Pasted%20image%2020231219142103.png)

Links to [Snowy ARMageddon](https://tryhackme.com/room/armageddon2r)

## Quest 3

This key will be hidden in one of the challenges of the main Advent of Cyber event between Day 9 and Day 16. Look for clues to find out which challenge to dig into!

```
Van Sprinkles left some stuff around the DC. It's like a secret message waiting to be unravelled!
```

![Advent of Cyber 2023 Side Quest 3 - Suspicious Files](tryhackme/images/aoc2023sq3_suspicious_files.png)


Downloading the files:

![Advent of Cyber 2023 Side Quest 3 - Download](tryhackme/images/aoc2023sq3_download.png)

## Quest 4

This key will be hidden in one of the challenges of the main Advent of Cyber event between Day 17 and Day 24. Look for clues to find out which challenge to dig into!

```

```