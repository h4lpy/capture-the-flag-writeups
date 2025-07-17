# Reaper

Our SIEM alerted us to a suspicious logon event which needs to be looked at immediately . The alert details were that the IP Address and the Source Workstation name were a mismatch .You are provided a network capture and event logs from the surrounding time around the incident timeframe. Corelate the given evidence and report back to your SOC Manager.

## Data

We are provided a `Reaper.zip` file:

- `Security.evtx`
- `ntlmrelay.pcapng`

### Walkthrough

Opening `ntlmrelay.pcapng` and looking at **Statistics->Endpoints**, we see a number of IP addresses in the 17.17.79.0/24 subnet range. As our first two tasks are to find IP addresses for `Forela-Wkstn001` and `Forela-Wkstn002`, we can filter the PCAP for `nbns` and look for refresh packets:

![](Pasted%20image%2020250717082521.png)

Based on the next set of questions, we can assume there was some kind of credential theft activity within the network. Looking through the capture, we see an odd series of events with `Forela-Wkstn002` attempting to access the `D` system:

![](Pasted%20image%2020250717084607.png)

As this system does not exist, the host then sends LLMNR queries across IPv4 and IPv6 with `172.17.79.135` responding as the answer:

![](Pasted%20image%2020250717084844.png)

This is a common NTLM relay technique through name resolution/LLMNR poisoning where the attacker attempts to hijack typos when users attempt to access UNC paths and respond to capture the user's hash - commonly NetNLTMv2 - and relay it to another host.

We can see the relay take place shortly after:

![](image-20240821074735072.webp)

- (1) `Forela-Wkstn002` sends LLMNR requests which the attacker's machine (`172.17.79.135`) is poisoning
- (2) `Forela-Wkstn002` establishes TCP connection to attacker on port 445
- (3) SMB authentication and request to access `\\D\IPC$`
- (4) Attacker attempts to establishes TCP connection to `Forela-Wkstn001` on port 445
- (5) TCP connection established
- (6) **NTLM relay**: attacker tells `Forela-Wkstn002` their session is expired, triggering a new authentication. Packets sent by `Forela-Wkstn002` to the attacker are relayed to `Forela-Wkstn001`, authenticating as `arthur.kyle`
	- The packets at the end are `Forela-Wkstn002` requesting access to `\\D\IPC$` which is typically the share SMB attempts to load when initialised before going to the actual requested share
	- We see the share name `\\172.17.79.29\IPC$` which the attacker attempted to access as part of the authentication process; this is inconsistent with the [Event ID 5140](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5140) in the `Security.evtx` logs which show the share as `\\*\IPC$`

![](Pasted%20image%2020250717090941.png)

Continuing with the PCAP analysis, we see the victim user on `Forela-Wkstn002` attempted to connect to the `\\DC01\Trip` share - likely the share they were attempting to connect to in the first instance - but the share does not exist.

![](Pasted%20image%2020250717091711.png)

Pivoting to the `Security.evtx` logs looking for Event ID 4624 and the attacker's IP, we see the successful login as `arthur.kyle` at `2024-07-31 04:55:16` over port `40252`

```
$ cat Security.json | jq 'select(.Event.System.EventID==4624 and .Event.EventData.IpAddress=="172.17.79.135") | .Event | {"Time": .System.TimeCreated["#attributes"].SystemTime, "SrcIP": .EventData.IpAddress, "SrcPort": .EventData.IpPort, "SrcHost": .EventData.WorkstationName, "Username": .EventData.TargetUserName, "LogonID": .EventData.TargetLogonId, "TargetHost": .System.Computer, "AuthPackage": .EventData.LmPackageName}'
{
  "Time": "2024-07-31T04:55:16.240589Z",
  "SrcIP": "172.17.79.135",
  "SrcPort": "40252",
  "SrcHost": "FORELA-WKSTN002",
  "Username": "arthur.kyle",
  "LogonID": "0x64a799",
  "TargetHost": "Forela-Wkstn001.forela.local",
  "AuthPackage": "NTLM V2"
}
```



## Tasks

1. What is the IP Address for `Forela-Wkstn001`?

Obtained by filtering PCAP for `nbns` for refresh packets:

```
172.17.79.129
```

2. What is the IP Address for `Forela-Wkstn002`?

Obtained by filtering PCAP for `nbns` for refresh packets:

```
172.17.79.136
```

3. What is the username of the account whose hash was stolen by attacker?

Found evidence of NTLM relay from compromised host `172.17.79.135` relaying packets from `Forela-Wkstn001` to `Forela-Wkstn002`, authenticating as `arthur.kyle`

```
arthur.kyle
```

4. What is the IP Address of Unknown Device used by the attacker to intercept credentials?

Found evidence of NTLM relay from compromised host, `172.17.79.135`, relaying packets from `Forela-Wkstn001` to `Forela-Wkstn002`, authenticating as `arthur.kyle`

```
172.17.79.135
```

5. What was the fileshare navigated by the victim user account?

Identified subsequent attempt by the vicitm on `Forela-Wkstn002` to access `\\DC01\Trip` - the share, however, does not exist

```
\\DC01\Trip
```

6. What is the source port used to logon to target workstation using the compromised account?

Queries `Security.evtx` file dumped by `evtx_dump` tool for EID 4624 and the attacker's IP:

```
$ cat Security.json | jq 'select(.Event.System.EventID==4624 and .Event.EventData.IpAddress=="172.17.79.135") | .Event | {"Time": .System.TimeCreated["#attributes"].SystemTime, "SrcIP": .EventData.IpAddress, "SrcPort": .EventData.IpPort, "SrcHost": .EventData.WorkstationName, "Username": .EventData.TargetUserName, "LogonID": .EventData.TargetLogonId, "TargetHost": .System.Computer, "AuthPackage": .EventData.LmPackageName}'
```

```
40252
```

7. What is the Logon ID for the malicious session?

See Q.6

```
0x64a799
```

8. The detection was based on the mismatch of hostname and the assigned IP Address. What is the workstation name and the source IP Address from which the malicious logon occur?

See Q.6

```
FORELA-WKSTN002,172.17.79.135
```

9. At what UTC time did the the malicious logon happen?

See Q.6

```
2024-07-31 04:55:16
```

10. What is the share Name accessed as part of the authentication process by the malicious tool used by the attacker?

Obtained through analysis of `Security.evtx` logs showing one EID 5140

```
\\*\IPC$
```