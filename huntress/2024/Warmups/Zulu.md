# Zulu

Did you know that zulu is part of the phonetic alphabet?

File: [zulu]()

-----

Checking the file type:

```
$ file zulu
zulu: compress'd data 16 bits
```

From the challenge name, Zulu in the phonetic alphabet is Z. Z is a  [compression algoritm](https://docs.fileformat.com/compression/z/) and is found within the UNIX operating system.

We can use [zcat](https://linux.die.net/man/1/uncompress) to decompress the content within the `zulu` file:

```
$ zcat zulu
flag{74235a9216ee609538022e6689b4de5c}
```

```
flag{74235a9216ee609538022e6689b4de5c}
```