# Specialer

Reception of Special has been cool to say the least. That's why we made an exclusive version of Special, called Secure Comprehensive Interface for Affecting Linux Empirically Rad, or just 'Specialer'. With Specialer, we really tried to remove the distractions from using a shell. Yes, we took out spell checker because of everybody's complaining. But we think you will be excited about our new, reduced feature set for keeping you focused on what needs it the most.

-----

Connecting to the instance via SSH shows we are in a shell environment stripped of commands like `ls` and `cat`.

However, we can use `cd` and `echo` and TAB completion to verify `/home/ctf-player` contains three directories:, `ala`, `abra`, and `sim`

Going through these directories with `cd`, we find a `kazam.txt` file in `ala/` which we can read with `echo`:

```
Specialer$ echo "$(<kazam.txt)"
return 0 picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_a8567b6f}
```

```
picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_a8567b6f}
```