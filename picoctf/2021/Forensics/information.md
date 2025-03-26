# information

Files can always be changed in a secret way. Can you find the flag? [cat.jpg](https://mercury.picoctf.net/static/149ab4b27d16922142a1e8381677d76f/cat.jpg)

-----

Running `exiftool` on the `cat.jpg` file to view the metadata shows a Base64-encoded string:

```
$ exiftool cat.jpg
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
```

Decoding the string reveals the flag:

```
$ echo 'cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9' | base64 -d
picoCTF{the_m3tadata_1s_modified}
```

```
picoCTF{the_m3tadata_1s_modified}
```