# Compromised WordPress

One of our WordPress sites has been compromised but we're currently unsure how. The primary hypothesis is that an installed plugin was vulnerable to a remote code execution vulnerability which gave an attacker access to the underlying operating system of the server.

-----

Provided `access.log`. Attacker IPs: `119.241.22.121`, `168.22.54.119`, `103.69.55.212`

1. Identify the URI of the admin login panel that the attacker gained access to (include the token)

In WordPress, the login panel is at `/wp-login.php`

```
$ cat access.log | grep -E '168.22.54.119|119.241.22.121|103.69.55.212' | grep '/wp-login.php?'
... /wp-login.php?itsec-hb-token=adminlogin ...
```

2. Can you find the two tools the attacker used?

Searching for common tools used against web applications/WordPress:

```
$ cat access.log | grep -E 'wpscan|sqlmap|wfuzz|gobuster|hydra|ffuf'
119.241.22.121 - - [14/Jan/2021:06:01:41 +0000] "GET / HTTP/1.1" 403 3160 "http://172.21.0.3/" "WPScan v3.8.10 (https://wpscan.org/)"
168.22.54.119 - - [14/Jan/2021:06:12:53 +0000] "POST /wp-login.php HTTP/1.1" 302 243 "-" "sqlmap/1.4.11#stable (http://sqlmap.org)"
```

```
WPScan SQLmap
```

3. The attacker tried to exploit a vulnerability in ‘Contact Form 7’. What CVE was the plugin vulnerable to? (Do some research!)

Research returns the following file upload vulnerability within the **Contact Form 7** plugin [CVE-2020-35489](https://blog.wpsec.com/contact-form-7-vulnerability/).

```
CVE-2020-35489
```

4. What plugin was exploited to get access?

The attacker uploaded a shell to `/uploads/simple-file-list`

```
Simple File List 4.2.2
```

5. What is the name of the PHP web shell file?

```
fr34k.php
```

6. What was the HTTP response code provided when the web shell was accessed for the final time?

```
404
```