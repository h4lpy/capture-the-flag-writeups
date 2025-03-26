# Little Shop of Hashes

_In the packet, secrets lie,  
Whispers of data pass by,  
Encrypted shadows creep,  
While the watchful eyes peep_.

Are you able to unravel the attack chain?

File: [little_shop_of_hashes_logs.zip]()

-----

**Little Shop of Hashes** was a Forensics challenge released on Day #13 of the Huntress Capture the Flag (CTF) competition. It comprised five parts derived from event logs collected from two hosts. We were tasked with investigating these logs and identify suspicious behaviour.

### (1) **What is the name of the service that the attacker ran and stopped, which dumped hashes on the first compromised host?**

In the Security logs, we observed a large number of Event ID (EID) [4776](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4776) and [4625](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4625) events within a short timeframe, which indicated a bruteforce attack.

![](https://miro.medium.com/v2/resize:fit:700/1*zz_3LK3mO9UD5WHgBOPxHQ.png)

Looking at the description for these events, we noted the target user was `DeeDee` and that the attack was occurring from a remote system with hostname `KALI` (IP: `10.1.1.42`).

![](https://miro.medium.com/v2/resize:fit:700/1*r6d8CErN-6zFVSZqg1lwQQ.png)

Following this, a successful logon to `DeeDee` occurred at `20:35:00`, as recorded by a EID [4624](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4624) event.

![](https://miro.medium.com/v2/resize:fit:700/1*9_BuwIOOLGKOB4Nc_jtArg.png)

At `20:36:47`, we then saw the threat actor launch the **RemoteRegistry** Windows service via `svchost.exe` (EID [4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688)) which would have allowed them to dump credentials for later use. This service is then teriminated at `20:36:50`, as evident by an EID [4689](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4689) event.

![](https://miro.medium.com/v2/resize:fit:700/1*HX-ktgtJZQM88b9vRUCvPA.png)

We correlated these times with Host B’s System logs which showed two **EID 7036** events:

![](https://miro.medium.com/v2/resize:fit:700/1*22qEGtgLihmj10dEGOyN5w.png)

```
Remote Registry
```

### (2) **What lateral movement technique did the threat actor use to move to the other machine?**

With the threat actor now in the posession of credentials dumped from the Windows Registry, we pivoted to Host A and searched for suspicious activity from the `10.1.1.42` IP address. This resulted in a EID [4624](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4624) event for the attacker logging in as `Craig` at `20:41:24`.

From carrying out further scoping, we observed a total of 16 EID [4624](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4624) from this IP, all of which were **network-based logons (Type 3) via NTLM**. Given that we know the threat actor to have already successfully compromised Host B and extracted credentials, this is likely **pass-the-hash [2]**.

```
Pass-the-hash
```

### (3) **What is the full path of the binary that the threat actor used to access the privileges of a different user with explicit credentials?**

Investigating the Security logs on Host A further, we saw another network login session from the threat actor at `20:47:20` for the `DeeDee` user. Immediately following this, we noted a process creation event (EID [4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688)) in which `wsmprovhost.exe` executed `runasc.exe` from the `DeeDee` user’s `Documents` folder using explicit credentials for `Niko`:

![](https://miro.medium.com/v2/resize:fit:700/1*oj1Ezg_PXPLakpbtGqCjtQ.png)

The parent process being `wsmprovhost.exe` shows the threat actor is using [PowerShell remoting](https://learn.microsoft.com/en-us/powershell/scripting/security/remoting/powershell-remoting-faq?view=powershell-7.4) to conduct reconnaissance as different users **[3]**.

```
C:\Users\DeeDee\Documents\runasc.exe
```

### (4) **How many accounts were compromised by the threat actor?**

Thus far, we have noted that the threat actor has compromised 3 users:

- `DeeDee`: via a brute force attack on Host B.
- `Craig`: via pass-the-hash using hashes dumped from Host B’s registry.
- `Victor`: via explicit credentials which were used to conduct reconnaissance via `runas.exe` on Host A.

```
3
```

### (5) **What is the full path of the binary that was used to attempt a callback to the threat actor’s machine?**

Finally, we reviewed Host A’s Application logs which showed multiple failures for `nc.exe` under EID 1109. Correlating this with the System logs, it confirmed this took place during the time the remote threat actor was logged in as `DeeDee` on the system, indicating they were likely attempting to callback to their system.

![](https://miro.medium.com/v2/resize:fit:700/1*QinfkbqH2V0vj9TUx-3eWQ.png)

```
C:\Users\DeeDee\Documents\nc.exe
```

### Summary

In this challenge we used Event Log Explorer to analyse the Application, Security, and System event logs of two hosts. We identified that a remote attacker was able to brute force the `DeeDee` user and dump registry contents from Host B which were subsequently used in a pass-the-hash attack on Host A.

Using these hashes, the attacker authenticated to Host A as both `Craig` and `DeeDee` and leveraged PowerShell Remoting to conduct further reconnaissance. This was facilitated via `runasc.exe` with Niko’s credentials. Finally, in a separate login session as `DeeDee`, the attacker attempted to make a callback to their own system via Netcat (`nc.exe`) but was unsuccessful due to OS incompatibility.

Below are some resources that were helpful during the investigation:

Ultimate Windows Security — Windows Event IDs_. [https://www.ultimatewindowssecurity.com/](https://www.ultimatewindowssecurity.com/)

Eviatar Gerzi — CyberArk (2017). _Detecting Pass-the-Hash with Windows Event Viewer_. [https://www.cyberark.com/resources/threat-research-blog/detecting-pass-the-hash-with-windows-event-viewer](https://www.cyberark.com/resources/threat-research-blog/detecting-pass-the-hash-with-windows-event-viewer)
Microsoft Documentation. _PowerShell Remoting_. [https://learn.microsoft.com/en-us/powershell/scripting/security/remoting/powershell-remoting-faq?view=powershell-7.4](https://learn.microsoft.com/en-us/powershell/scripting/security/remoting/powershell-remoting-faq?view=powershell-7.4)