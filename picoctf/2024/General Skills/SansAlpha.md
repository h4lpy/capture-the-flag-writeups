# SansAlpha

The Multiverse is within your grasp! Unfortunately, the server that contains the secrets of the multiverse is in a universe where keyboards only have numbers and (most) symbols.

-----

Connecting to the instance via `ssh`, we quickly confirm that we cannot use any commands with letters, only numbers and symbols.

Firstly, we can set a variable to get characters which we can use wildcards and substrings to access:

```
SansAlpha$ _1=`$ 2>&1`
```

So when `$_1` is executed, we can use any character within the `bash: bash:: command not found` string.

Using `./*` to view directory contents, we find the flag is in `./blargh/flag.txt`:

```
SansAlpha$ ./*/*
bash: ./blargh/flag.txt: Permission denied
```

Using our variable, we can use substrings and `/???` wildcards to get the flag:

```
SansAlpha$ /???/?${_1:9:1}?${_1:10:1} "$(<./*/????.???)"
return 0 picoCTF{7h15_mu171v3r53_15_m4dn355_775ac12d}
```