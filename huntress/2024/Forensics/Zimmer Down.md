# Zimmer Down

A user interacted with a suspicious file on one of our hosts.  
The only thing we managed to grab was the user's registry hive.  
Are they hiding any secrets?

File: [NTUSER.DAT]()

-----

Zimmer Down was a Forensics challenge released on Day #8 of the Huntress Labs Capture the Flag (CTF) competition. We were provided an `NTUSER.DAT` file and tasked with finding a suspicious file that the user interacted with.

Given the challenge name, we opened the using [Eric Zimmerman’s Registry Explorer](https://ericzimmerman.github.io/#!index.md) to examine its contents. The `NTUSER.DAT` file stores user-specific settings and configurations located within the user’s profile folder. In digital forensics, it can provide valuable information about user activity, particularly in relation to files that may have been accessed. When combined with other artifacts, this information can help prove how a user behaved on the system in question.

For the scope of this challenge, we focused on files and folders that the user had accessed. There are various ways to track this information, but it is most commonly recorded by the `RecentDocs` registry key located at `\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`, grouped by file extension.

![](https://cdn-images-1.medium.com/max/800/1*BcTrst8iwaLd9b87hSRnng.png)

In Registry Explorer, we noted 28 entries across 12 extensions, the first of which, `.b62` , contained an unusual file `VJGSuERgCoVhl6mJg1x87faFOPIqacI3Eby4oP5MyBYKQy5paDF[.]b62` which was opened on **2024–10–02 02:47:01**.

![](https://cdn-images-1.medium.com/max/800/1*Cxriy3otM78B6pvTiTxvuA.png)

Taking the name of this file and assuming that the extension indicated it was encoded with Base62, we decoded it to retrieve the flag.

![](https://cdn-images-1.medium.com/max/800/1*CEDN-0VbflqVzpIylAqD5w.png)

```
flag{4b676ccc1070be66b1a15dB601c8d500}
```