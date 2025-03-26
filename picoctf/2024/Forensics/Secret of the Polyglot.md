# Secret of the Polyglot

The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file? Download the suspicious file [here](https://artifacts.picoctf.net/c_titan/98/flag2of2-final.pdf).

-----

Opening the file, we see the second part of the flag:

```
1n_pn9_&_pdf_1f991f77}
```

Looking at the file with `file`, we see it is a PNG:

```
$ file flag2of2-final.pdf
flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
```

Changing the extension to `.png`, we get the first part of the flag:

```
picoCTF{f1u3n7_
```

Flag:

```
picoCTF{f1u3n7_1n_pn9_&_pdf_1f991f77}
```