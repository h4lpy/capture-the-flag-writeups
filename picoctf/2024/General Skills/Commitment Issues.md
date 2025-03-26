# Commitment Issues

I accidentally wrote the flag down. Good thing I deleted it! You download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/136/challenge.zip)

-----

Unzipping the provided archive, we are given a `message.txt` file which contains:

```
$ cat message.txt
TOP SECRET
```

We are also given a `.git` directory which we can view the commit history for with `git log`:

```
$ git log
commit 8dc51806c760dfdbb34b33a2008926d3d8e8ad49 (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:06:17 2024 +0000

    remove sensitive info

commit 87b85d7dfb839b077678611280fa023d76e017b8
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:06:17 2024 +0000

    create flag
```

From the above, we see commit `87b85d7dfb839b077678611280fa023d76e017b8` by `picoCTF` had the message `create flag`. Checking out this commit, we can get the flag:

```
$ git checkout 87b85d7dfb839b077678611280fa023d76e017b8
$ cat message.txt
picoCTF{s@n1t1z3_ea83ff2a}
```