# So Meta

Find the flag in this [picture](https://jupiter.challenges.picoctf.org/static/00efdf2961da1e21470ffc0d496c3cc2/pico_img.png).

-----

Opening the image shows nothing of concern:

![[picoctf/images/Pasted image 20240305195331.png]]

Running `strings` and `exiftool` returns the flag:

```console
$ exiftool pico_img.png
...
Artist                          : picoCTF{s0_m3ta_fec06741}
```

```
picoCTF{s0_m3ta_fec06741}
```