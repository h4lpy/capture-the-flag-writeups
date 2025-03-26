# I Can't SSH

I've got this private key... but why can't I SSH?

File: [id_rsa]()

-----

Attempting to connect to the instance with the provided `id_rsa` file results in error.

```
$ ssh -i id_rsa -p [port] user@challenge.ctf.games
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0664 for 'id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "id_rsa": bad permissions
user@challenge.ctf.games's password: 
```

This indicates the permissions of the file are too permissive, so we must change them via `chmod`:

```
$ chmod 600 id_rsa
```

However, this now produces a different error:

```
Load key "id_rsa": error in libcrypto
```

Examining the key file, we see it contains improper line endings and is absent a new line at the end. Adding in this new line and running `dos2unix`, we can convert the key file into the correct format.

```
$ dos2unix id_rsa
```

With this file, we can retrieve the flag once connected to the instance:

```
$ ssh -i id_rsa -p [port] user@challenge.ctf.games
```

We can then retrieve the flag from the user's `/home` directory:

```
$ cat flag.txt
flag{ee1f28722ec1ce1542aa1b486dbb1361}
```

```
flag{ee1f28722ec1ce1542aa1b486dbb1361}
```

