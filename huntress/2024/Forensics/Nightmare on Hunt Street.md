# Nightmare on Hunt Street

_DeeDee hears the screams,  
In the logs, a chilling trace—  
Freddy's waiting near._

File: [logs-parts1-5.zip]()

-----

Nightmare on Hunt Street was a Forensics challenge released on Day #3 of the Huntress Labs Capture the Flag (CTF) competition. It comprised five parts derived from three provided files: `Application.evtx`, `Security.evtx`, and `System.evtx`.

Upon opening the files in Event Log Explorer, we found `Application.evtx` to be empty, leaving us just to investigate the Security and System logs.

### (1) What is the IP address of the host that the attacker used?

In the Security logs, we observed a large number of Event ID (EID) [4776](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4776) and [4625](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4625) events within a short timeframe, which indicated a bruteforce attack. The target user was consistently identified as `Jsmith` .

![](https://cdn-images-1.medium.com/max/800/1*utuqpz7kDVDDBqfRkbXDdw.png)

Sample of EID 4776 and 4625 corresponding to a brute force attack

From EID [4625](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4625), we noted that the remote hostname was `KALI`, while EID [4776](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4776) events attributed the IP address `10.1.1.42` to this system:

![](https://cdn-images-1.medium.com/max/800/1*xNdMEFO9ZnxmLijAGeqG3Q.png)

EID 4776 shows remote threat actor’s hostname is “KALI”

![](https://cdn-images-1.medium.com/max/800/1*1xFt-ejr_OX3xQ3_LjRjQg.png)

EID 4625 shows the source IP address is 10.1.1.42

> 10.1.1.42

### (2) How many times was the compromised account brute-forced?

There were three major spikes in events between 2024–09–24 12:06:30 to 2024–09–24 12:07:10 resulting in a total of 65 events across both EID [4776](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4776) and EID [4625](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4625).

Filtering specifically for EID [4625](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4625), which corresponds to “An account failed to log on”, we get `32` attempts to bruteforce the `Jsmith` account. A successful logon occurred at 2024–09–24 21:07:10, as recorded by a EID [4624](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4624) event.

![](https://cdn-images-1.medium.com/max/800/1*bK9EA9AKmsTIPioLx7jdPw.png)

EID 4624 showing the threat actor successfully logged in as Jsmith

> 32

### (3) What is the name of the offensive security tool that was used to gain initial access?

Pivoting to the System logs, we noticed two EID 7045 events, indicating a service installation on the system. Both events were linked to the Security Identifier (SID) `S-1–5–21–3792037069–2078677046–3387239386–1000` which belongs to `Jsmith` .

These events also occurred after the successful logon by the attacker at 2024–09–24 21:08:18 and 2024–09–24 21:11:18 and were treated with suspicion, as the `Jsmith` user was known to be compromised.

![](https://cdn-images-1.medium.com/max/800/1*Pmqkwbs4T42VoIcktWAxOQ.png)

Cross-correlating EID 4624 and 7045 confirms the service creation aligns to Jsmith

Examining the service creation events, we found that the executable names were randomised 8-character strings: `MrEQbpfX[.]exe` and `wgWMRHln[.]exe` . This service names were similarly randomised, with 4-characters names: `WREx` and `fdpa` .

Research into these patterns confirmed the attack tool used was [Impacket](https://github.com/fortra/impacket), specifically `PSEXEC` which creates and subsequently deletes a Windows service with a random 4-character mixed-case name referencing an 8-character mixed-case name within `%SystemRoot%` [1].

![](https://cdn-images-1.medium.com/max/800/1*q1ZhwLayshjri6DpvXh4vA.png)

Naming conventions of installed services are confirmed to be the result of impacket-psexec execution

> impacket-psexec

### (4) How many unique enumeration commands were run with `net.exe`?

Returning to the Security logs, we filtered for “`net.exe`” starting from the logon time of `Jsmith` . Several EID [4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688) events aligned to process creation revealed the following enumeration commands:

net user  
net localgroup  
net share

These commands enumerated local useres, local groups, and available shares on the system, confirming further malicious activity after `Jsmith` was compromised.

> 3

### (5) What password was successfully given to the user created?

Further investigation of EID [4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688) events following the enumeration activity revealed the creation of a new user at 2024–09–24 21:09:49:

![](https://cdn-images-1.medium.com/max/800/1*P2N149pRpdjs4NAz5LgyPg.png)

EID 4688 show the creation of a new administrator user

The new user was named `susan_admin`, with the password `SusanIsStrong123` .

> SusanIsStrong123

### Summary

In this challenge, we used Event Log Explorer to identify a remote attacker who had brute-forced the `Jsmith` user’s credentials. The attacker leveraged Impacket’s PSEXEC module to gain initial access into the environment and performed further reconnaissance using `net.exe` commands to enumerate local users, groups, and shares. Finally, the attacker added a high-privileged user, `susan_admin` , for persistence.