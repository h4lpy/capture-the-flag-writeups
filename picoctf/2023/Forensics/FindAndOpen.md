# FindAndOpen

Someone might have hidden the password in the trace file. Find the key to unlock [this file](https://artifacts.picoctf.net/c/492/flag.zip). [This tracefile](https://artifacts.picoctf.net/c/492/dump.pcap) might be good to analyze.

-----

We are given two files, `flag.zip` and `dump.pcap`.

Running `strings`, we get the following output:

```
iBwaWNvQ1RGe1Could the flag have been splitted?
AABBHHPJGTFRLKVGhpcyBpcyB0aGUgc2VjcmV0OiBwaWNvQ1RGe1IzNERJTkdfTE9LZF8=
PBwaWUvQ1RGesabababkjaASKBKSBACVVAVSDDSSSSDSKJBJS
PBwaWUvQ1RGe1Maybe try checking the other file
```

Extracting the encoded pieces:

```
iBwaWNvQ1RGe1
PBwaWUvQ1RGesabababkjaASKBKSBACVVAVSDDSSSSDSKJBJS
AABBHHPJGTFRLKVGhpcyBpcyB0aGUgc2VjcmV0OiBwaWNvQ1RGe1IzNERJTkdfTE9LZF8=
```

Rearranging:

```
iBwaWNvQ1RGe1PBwaWUvQ1RGesabababkjaASKBKSBACVVAVSDDSSSSDSKJBJSAABBHHPJGTFRLKVGhpcyBpcyB0aGUgc2VjcmV0OiBwaWNvQ1RGe1IzNERJTkdfTE9LZF8=
```

```
This is the secret: picoCTF{R34DING_LOKd_
```

Entering `picoCTF{R34DING_LOKd_` as the password to `flag.zip` returns the flag:

```
picoCTF{R34DING_LOKd_fil56_succ3ss_cbf2ebf6}
```