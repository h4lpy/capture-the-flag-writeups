# Base64by32

This is a dumb challenge. I'm sorry.

File: [base64by32.zip]()

-----

Unzipping the archive, we are given a text file, `base64by32`, with a large Base64-encoded string. From the challenge name, this has been encoded 32 times, so we can decode it as follows:

```bash
#!/bin/bash

result=$(cat base64by32)
for i in {1..32}; do result=$(echo "$result" | base64 -d); done; echo "$result"
```

```
flag{8b3980f3d33f2ad2f531f5365d0e3970}
```