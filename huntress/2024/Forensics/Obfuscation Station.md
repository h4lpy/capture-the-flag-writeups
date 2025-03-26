# Obfuscation Station

You've reached the Obfuscation Station!  
Can you decode this PowerShell to find the flag?  
**Archive password: `infected-station`**

File: [Challenge.zip]()

-----

**Obfuscation Station** was a Forensics challenge released on Day #13 of the Huntress Labs Capture the Flag (CTF) competition. We were provided a Zip archive containing a PowerShell script, `chal.ps1`, and tasked with decoding the script to find the flag.

Opening the file showed a single line of PowerShell with a calls to IO modules which indicate the string is Base64-encoded and compressed:

![](https://miro.medium.com/v2/resize:fit:700/1*kaUnUhk_Tp__AvwsI9PFcg.png)

We then used Python to reverse the encoding and compression to get the flag:

```python
import zlib  
import base64  
  
base64_string = "UzF19/UJV7BVUErLSUyvNk5NMTM3TU0zMDYxNjSxNDcyNjexTDY2SUu0NDRITDWpVQIA"  
  
compressed_data = base64.b64decode(base64_string)  
  
# 'wbits=-zlib.MAX_WBITS' needed when a raw deflate stream is used  
decompressed_data = zlib.decompress(compressed_data, wbits=-zlib.MAX_WBITS)  
  
original_text = decompressed_data.decode('ascii')  
  
print(original_text)
```

![](https://miro.medium.com/v2/resize:fit:519/1*DRF9zcDz3xVkPTh3ZxTsyQ.png)

```
flag{3ed675ef0343149723749c34fa910ae4}
```