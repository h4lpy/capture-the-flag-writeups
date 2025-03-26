# Invisible WORDs

Do you recognize this cyberpunk baddie? We don't either. AI art generators are all the rage nowadays, which makes it hard to get a reliable known cover image. But we know you'll figure it out. The suspect is believed to be trafficking in classics. That probably won't help crack the stego, but we hope it will give motivation to bring this criminal to justice! Download the image [here](https://artifacts.picoctf.net/c/402/output.bmp)

-----

We are given a BMP file, `output.bmp`:

![[picoctf/images/output.bmp]]

```
$ file output.bmp
output.bmp: PC bitmap, Windows 98/2000 and newer format, 960 x 540 x 32, cbSize 2073738, bits offset 138
```

Based on the challenge name, this is based on [words](https://en.wikipedia.org/wiki/Word_(computer_architecture)) which, in computer science, are typically stored as 2 bytes (single word - 16 bits), 4 bytes (double word - 32 bytes), or even 8 bytes (quad word - 128 bytes).

Using [CyberChef's Randomize Colour Palette recipe](https://gchq.github.io/CyberChef/#recipe=Randomize_Colour_Palette('')), we see the bottom section of pixels below the centre image is slightly different in colour to the top section:

![[picoctf/images/Untitled.bmp]]

Looking at the hex dump of the image, we see `PK` bytes (offset `0x8c`) which indicate a Zip file is hidden:

![[picoctf/images/Pasted image 20250225105542.png]]

However, looking at the [expected magic bytes](https://en.wikipedia.org/wiki/List_of_file_signatures), the next two bytes should be `03 04` (offset `0x90`). These bytes are actually offset two bytes ahead:

![[picoctf/images/Pasted image 20250225105818.png]]

To extract the PKZip file, we also need the central directory record which is denoted by `50 4b 01 02` and the end of the directory record which is denoted by `50 4b 05 06`. We can find these with `binwalk`:

```
$ binwalk -R "\x50\x4b" output.bmp

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
140           0x8C            Raw signature (\x50\x4b)
149837        0x2494D         Raw signature (\x50\x4b)
181988        0x2C6E4         Raw signature (\x50\x4b)
307213        0x4B00D         Raw signature (\x50\x4b)
308596        0x4B574         Raw signature (\x50\x4b)
339092        0x52C94         Raw signature (\x50\x4b)
339288        0x52D58         Raw signature (\x50\x4b)
```

However, as we know the bytes of the signature were split, these bytes will likely be split also. As such, we can create a Python script to remove every second pair of bytes so that we are left with the PKZip file:

```
# Place each byte on its own line
$ xxd -p -c 1 output.bmp > hex.dump
```

```python
#!/usr/bin/env python3

bytes = []

f = open('hex.dump', 'r')

bytes = f.read().split()

f.close()

pairing1 = bytes[0::4]  # Every 4th byte into list, starting at index 0
pairing2 = bytes[1::4]  # Every 4th byte into list, starting at index 1

new_bytes = ''

for f,b in zip(pairing1, pairing2):
	new_bytes += f
	new_bytes += b

print(new_bytes)
```

We can run this and then truncate the output:

```
$ python3 truncate_hex.py | xxd -r -p > hex_trunc
```

We now have the `05 4b 03 04` bytes in the correct location:

![[picoctf/images/Pasted image 20250225111337.png]]

With `binwalk`, we can also see that we have a Zip file with a `ZnJhbmtlbnN0ZWluLXRlc3QudHh0` file:

```
$ binwalk hex_trunc

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
70            0x46            Zip archive data, at least v2.0 to extract, compressed size: 169390, uncompressed size: 448642, name: ZnJhbmtlbnN0ZWluLXRlc3QudHh0
169644        0x296AC         End of Zip archive, footer length: 22
```

Extracting this with `binwalk -e` and reading its contents returns the flag:

```
$ cat _hex_trunc.extracted/ZnJhbmtlbnN0ZWluLXRlc3QudHh0 | grep pico
At that age I became acquainted with the celebrated picoCTF{w0rd_d4wg_y0u_f0und_5h3113ys_m4573rp13c3_5eadc23e}
```

```
picoCTF{w0rd_d4wg_y0u_f0und_5h3113ys_m4573rp13c3_5eadc23e}
```