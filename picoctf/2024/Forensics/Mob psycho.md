# Mob psycho

Can you handle APKs? Download the android apk [here](https://artifacts.picoctf.net/c_titan/140/mobpsycho.apk).

-----

We can unzip the provided `mobpsycho.apk` file as we would any Zip archive to unpack all of its resources:

```
$ file mobpsycho.apk
mobpsycho.apk: Zip archive data, at least v1.0 to extract, compression method=store
$ unzip mobpsycho.apk
```

With the unpacked files, we can use `find` to locate the `flag.txt` file:

```
$ find . -type f -name "*flag*" 2>/dev/null
./res/color/flag.txt
```

The contents of the file are encoded with hex:

```
$ cat ./res/color/flag.txt | hex -d     
picoCTF{ax8mC0RU6ve_NX85l4ax8mCl_5e67ea5e}%   
```