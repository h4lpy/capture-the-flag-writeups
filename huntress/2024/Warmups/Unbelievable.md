# Unbelievable

Don't believe everything you see on the Internet!  
  
Anyway, have you heard this intro soundtrack from Half-Life 3?

File: [Half-Life_3_OST.mp3](obsidian://open?vault=capturetheflags&file=huntress%2F2024%2FWarmups%2Fchallenge-files%2FHalf-Life_3_OST.mp3)

-----

Running `file`, we see the file is not an `mp3` but rather a PNG:

```
$ file Half-Life_3_OST.mp3
Half-Life_3_OST.mp3: PNG image data, 800 x 200, 8-bit/color RGB, non-interlaced
```

Changing the file extension and opening it reveals the flag:

![[huntress/2024/images/Half-Life_3_OST.png]]

```
flag{a85466991f0a8dc3d9837a5c32fa0c91}
```