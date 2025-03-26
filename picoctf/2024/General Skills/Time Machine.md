# Time Machine

What was I last working on? I remember writing a note to help me remember... You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/161/challenge.zip)

-----

Unzipping the provided archive, we get a `message.txt` file which reads:

```
This is what I was working on, but I'd need to look at my commit history to know why...
```

Also included in the directory is a `.git` directory which we can view the commit history of with `git log`:

```
$ git log
...
commit 10228f3d6437701ef5aaac04213757031f30ebec (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:24 2024 +0000

    picoCTF{t1m3m@ch1n3_8defe16a}
```