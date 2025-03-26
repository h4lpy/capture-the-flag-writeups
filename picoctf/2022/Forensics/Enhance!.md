# Enhance!

Download this image file and find the flag.

- [Download image file](https://artifacts.picoctf.net/c/100/drawing.flag.svg)

-----

We are given an SVG file:

```
$ file drawing.flag.svg
drawing.flag.svg: SVG Scalable Vector Graphics image
```

![[picoctf/images/drawing.flag.svg]]

The flag is contained within the `<tspan></tspan>` attributes:

```
$ strings drawing.flag.svg | grep "id=\"tspan3"
         id="tspan3748">p </tspan><tspan
         id="tspan3754">i </tspan><tspan
         id="tspan3756">c </tspan><tspan
         id="tspan3758">o </tspan><tspan
         id="tspan3760">C </tspan><tspan
         id="tspan3762">T </tspan><tspan
         id="tspan3764">F { 3 n h 4 n </tspan><tspan
         id="tspan3752">c 3 d _ a a b 7 2 9 d d }</tspan></text>
```

```
$ strings drawing.flag.svg | grep "id=\"tspan3" | cut -d ">" -f 2 | cut -d "<" -f 1 | tr -d " \t\n\r"
picoCTF{3nh4nc3d_aab729dd}
```

```
picoCTF{3nh4nc3d_aab729dd}
```