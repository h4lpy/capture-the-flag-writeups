# interencdec

Can you get the real meaning from this file. Download the file [here](https://artifacts.picoctf.net/c_titan/2/enc_flag).

-----

Viewing the contents of the file:

```
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6YzRNalV3YUcxcWZRPT0nCg==
```

We can decode this with `base64`:

```
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzc4MjUwaG1qfQ=='
```

The string within `b''` is also Base64 encoded which results in a ROT encoded string when decoded:

```
wpjvJAM{jhlzhy_k3jy9wa3k_78250hmj}%
```

Using [CyberChef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,19)&input=d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzc4MjUwaG1qfQ) to decode the string returns the flag:

```
picoCTF{caesar_d3cr9pt3d_78250afc}
```

-----

Can use single command to get to ROT19 encoded:

```
$ cat enc_flag | base64 -d | cut -d "'" -f2 | base64 -d
```