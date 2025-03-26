# Typo

Gosh darnit, I keep entering a typo in my Linux command prompt!

-----

Connecting to the instance via SSH, we are shown an ASCII-art train animation:

![[huntress/2024/images/typo_train.gif]]

When the animation finishes, our SSH session dies.

With SSH, we are able to pass another command-line argument which runs commands on the destination host when connected. As such, we can run `cat flag.txt` to retrieve the flag:

```
$ ssh -p [port] user@challenge.ctf.games "cat flag.txt"
```

```
flag{36a0354fbf59df454596660742bf09eb}
```