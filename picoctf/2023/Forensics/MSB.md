# MSB

This image passes LSB statistical analysis, but we can't help but think there must be something to the visual artifacts present in this image... Download the image [here](https://artifacts.picoctf.net/c/301/Ninja-and-Prince-Genji-Ukiyoe-Utagawa-Kunisada.flag.png)

-----

![[picoctf/images/Ninja-and-Prince-Genji-Ukiyoe-Utagawa-Kunisada.flag.png]]

We are given the above image with the challenge prompt suggesting that data may be hidden in the most significant bit (MSB) of the RGB pixels in the image.

As such, we can use a tool to extract these bits - [sigBits.py](https://raw.githubusercontent.com/Pulho/sigBits/master/sigBits.py).

```
$ wget https://raw.githubusercontent.com/Pulho/sigBits/master/sigBits.py
$ python3 sigBits.py -t=msb Ninja-and-Prince-Genji-Ukiyoe-Utagawa-Kunisada.flag.png
```

This outputs a `outputSB.txt` file which contains the flag:

```
picoCTF{15_y0ur_que57_qu1x071c_0r_h3r01c_3a219174}
```