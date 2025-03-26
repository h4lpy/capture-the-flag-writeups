# Noted | Easy

#notepadplusplus #osint #cachedfiles

Simon, a developer working at Forela, notified the CERT team about a note that appeared on his desktop. The note claimed that his system had been compromised and that sensitive data from Simon's workstation had been collected. The perpetrators performed data extortion on his workstation and are now threatening to release the data on the dark web unless their demands are met. Simon's workstation contained multiple sensitive files, including planned software projects, internal development plans, and application codebases. The threat intelligence team believes that the threat actor made some mistakes, but they have not found any way to contact the threat actors. The company's stakeholders are insisting that this incident be resolved and all sensitive data be recovered. They demand that under no circumstances should the data be leaked. As our junior security analyst, you have been assigned a specific type of DFIR (Digital Forensics and Incident Response) investigation in this case. The CERT lead, after triaging the workstation, has provided you with only the Notepad++ artifacts, suspecting that the attacker created the extortion note and conducted other activities with hands-on keyboard access. Your duty is to determine how the attack occurred and find a way to contact the threat actors, as they accidentally locked out their own contact information. Warning : This sherlock requires an element of OSINT and players will need to interact with 3rd party services on internet.

## Walkthrough

Extracting the files from `Noted[.]zip`, we find four files within `C/Users/Simon.stark/AppData/Roaming/Notepad++`:

- `backup/`
	- `LootAndPurge[.]java[@]2023-07-24_145332`
	- `YOU HAVE BEEN HACKED[.]txt[@]2023-07-24_150548`
- `config[.]xml`
- `session[.]xml`

Opening `config[.]xml`, we see various files present in the history, most notably **the script used by Simon for AWS operations** - `AWS_objects migration[.]pl`:

![[Pasted image 20240401183043.png]]

Checking `session[.]xml`, we see both the `LootAndPurge[.]java` and `YOU HAVE BEEN HACKED[.]txt` files were being worked on with the original file paths being observed in the user's Desktop - `C:\Users\simon.stark\Desktop\...`

![[Pasted image 20240401183347.png]]

Taking a closer look at the low and high timestamps for the `LootAndPurge[.]java` file, we can extract the last modified time:

```
originalFileLastModifTimestamp="-1354503710"
originalFileLastModifTimestampHigh="31047188"
```

```python
#!/usr/bin/env python3

import datetime

original_file_last_modified_timestamp = -1354503710
original_file_last_modified_timestamp_high = 31047188

full_timestamp = (original_file_last_modified_timestamp_high << 32) | (original_file_last_modified_timestamp & 0xFFFFFFFF)

timestamp_seconds = full_timestamp / 10**7

timestamp = datetime.datetime(1601, 1, 1) + datetime.timedelta(seconds=timestamp_seconds)

print(f'Last modified timestamp: {timestamp}')
```

Pivoting to the `YOU HAVE BEEN HACKED[.]txt` file, a link to a password-protected Pastebin is given - `hxxps[://]pastebin[.]com/CwhBVzPq`.

![[Pasted image 20240401184202.png]]

The password is given in cleartext within the `LootAndPurge[.]java` script, allowing us to access the Pastebin and retrieve the **Ethereum wallet address** and **email contact info**.

![[Pasted image 20240401184241.png]]

![[Pasted image 20240401184423.png]]

---

1. What is the full path of the script used by Simon for AWS operations?

```
C:\Users\Simon.stark\Documents\Dev_Ops\AWS_objects migration[.]pl
```

2. The attacker duplicated some program code and compiled it on the system, knowing that the victim was a software engineer and had all the necessary utilities. They did this to blend into the environment and didn't bring any of their tools. This code gathered sensitive data and prepared it for exfiltration. What is the full path of the program's source file?

```
C:\Users\simon.stark\Desktop\LootAndPurge[.]java
```

3. What's the name of the final archive file containing all the data to be exfiltrated?

```
Forela-Dev-Data[.]zip
```

4. What's the timestamp in UTC when attacker last modified the program source file?

```
2023-07-24 09:53:23
```

5. The attacker wrote a data extortion note after exfiltrating data. What is the crypto wallet address to which attackers demanded payment?

```
0xca8fa8f0b631ecdb18cda619c4fc9d197c8affca
```

6. What's the email address of the person to contact for support?

```
CyberJunkie[@]mail2torjgmxgexntbrmhvgluavhj7ouul5yar6ylbvjkxwqf6ixkwyd[.]onion
```

-----
