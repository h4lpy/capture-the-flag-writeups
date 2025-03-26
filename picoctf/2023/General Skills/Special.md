# Special

Don't power users get tired of making spelling mistakes in the shell? Not anymore! Enter Special, the Spell Checked Interface for Affecting Linux. Now, every word is properly spelled and capitalized... automatically and behind-the-scenes! Be the first to test Special in beta, and feel free to tell us all about how Special streamlines every development process that you face. When your co-workers see your amazing shell interface, just tell them: That's Special (TM)

-----

Connecting to the instance via SSH, we see a custom bash shell which takes our commands and checks them for spelling.

We can use double parentheses to get commands to execute:

```
Special$ ((ls))
((ls))
blargh
```

More parentheses will confirm `blargh` is a directory with `flag.txt`:

```
Special$ ((((ls blargh))))
((((ls blargh))))
flag.txt
```

We can read the flag in the same way:

```
Special$ ((((cat blargh/flag.txt))))
((((cat blargh/flag.txt))))
picoCTF{5p311ch3ck_15_7h3_w0r57_0c61d335}
```

```
picoCTF{5p311ch3ck_15_7h3_w0r57_0c61d335}
```