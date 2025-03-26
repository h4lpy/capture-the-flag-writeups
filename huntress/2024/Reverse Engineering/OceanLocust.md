# OceanLocust
  
_Wow-ee zow-ee!!_ Some advanced persistent threats have been doing some tricks with hiding payloads in image files!  
  
We thought we would try our hand at it too.

File: [ocean_locust.7z]()

-----

For this challenge, we are given a 7zip archive, `ocean_locust.7z`, which contains three files:

- `inconspicious.png`
- `png-challenge.exe`: Stripped of debug symbols (harder route)
- `png-challenge-debug.exe`: Contains debug symbols (easier route)



We noticed a recurring pattern throughout the PNG

```
flag{fec87c690b8ec8d65b8bb10ee7bb65d0}
```