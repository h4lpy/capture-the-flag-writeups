# Hidden Streams

Beneath the surface, secrets glide,  
A gentle flow where whispers hide.  
Unseen currents, silent dreams,  
Carrying tales in hidden streams.  
  
Can you find the secrets in these Sysmon logs?

File: [Challenge.zip]()

-----

**Hidden Streams** was a Forensics challenge released on Day #14 of the Huntress Labs Capture the Flag (CTF) competition. We were given a Zip file containing Sysmon logs (`Sysmon.evtx`) and tasked with finding the flag within these events.

After opening the event logs within **Event Log Explorer**, we discovered a total of **2850 events** recorded between `2024-08-28 00:18:19 UTC` and `2024-08-28 00:20:49 UTC`.

By referencing the documentation for [Sysmon Event IDs](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon), we focused on [Event ID (EID) 15](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) — `FileCreateStreamHash`. This EID is triggered when a named file stream is created, logging the file’s content. This is commonly observed in [Alternate Data Streams (ADS)](https://www.sans.org/blog/alternate-data-streams-overview/), such as `Zone.Identifier`, which is created when files are downloaded via a web browser.

Filtering for this EID resulted in one event for the ADS `C:\Temp:$5GMLW`.

![](https://cdn-images-1.medium.com/max/800/1*Aqe562UeLhqFds-LxKOsSw.png)

The content of this ADS was a Base64-encoded string:

```
ZmxhZ3tiZmVmYjg5MTE4MzAzMmY0NGZhOTNkMGM3YmQ0MGRhOX0=
```

We decoded this to obtain the flag:

![](https://cdn-images-1.medium.com/max/800/1*Frer2KVRmSeBLnt9GnUeOg.png)

```
flag{bfefb891183032f44fa93d0c7bd40da9}
```