# CanYouSee

How about some hide and seek? Download this file [here](https://artifacts.picoctf.net/c_titan/129/unknown.zip).

-----

Unzipping the provided archive and running `exiftool` to view the metadata reveals a Base64-encoded string:

```
$ exiftool ukn_reality.jpg
...
Attribution URL                 : cGljb0NURntNRTc0RDQ3QV9ISUREM05fYjMyMDQwYjh9Cg==
...
```

Decoding the Base64 reveals the flag:

```
$ echo 'cGljb0NURntNRTc0RDQ3QV9ISUREM05fYjMyMDQwYjh9Cg==' | base64 -d
picoCTF{ME74D47A_HIDD3N_b32040b8}
```