# Scan Surprise

#### Description

I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead? You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_atlas/2/challenge.zip)

The same files are accessible via SSH here: `ssh -p 54761 ctf-player@atlas.picoctf.net` Using the password `1ad5be0d`. Accept the fingerprint with `yes`, and `ls` once connected to begin. Remember, in a shell, passwords are hidden!

-----

Using `zbarimg` to view the contents of the `flag.png` QR code:

```
$ zbarimg -q flag.png                                          
QR-Code:picoCTF{p33k_@_b00_b5ce2572}
```