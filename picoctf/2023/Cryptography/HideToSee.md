# HideToSee

How about some hide and seek heh? Look at this image [here](https://artifacts.picoctf.net/c/236/atbash.jpg).

-----

We are given a `atbash.jpg` image which contains something.

Using `steghide`, we can extract this with no password:

```
$ steghide extract -sf atbash.jpg
Enter passphrase:
wrote extracted data to "encrypted.txt".
```

This extracts a `encrypted.txt` file containing the following, encoded with [AtBash](https://www.dcode.fr/atbash-cipher)

```
krxlXGU{zgyzhs_xizxp_zx751vx6}
```

We can decode it with [Dcode.fr - AtBash](https://www.dcode.fr/atbash-cipher) to get the flag:

```
picoCTF{atbash_crack_ac751ec6}
```