# Cool Runnings

You can hide everything during winter!

-----

We are given two files, `SD.txt` and a password-protected `Locked.rar`:

```
Happy	     	       	      	    	       	    	   	 	   
Sleepy    	  	      		  	       	  	  
Dopey	
Sneezy
Grumpy
Bashful
Doc

```

- Used `stegsnow` to get password `Blizzard` for `Locked.rar` from whitespace in `SD.txt file`
- `Locked.rar` contains `broken.png`, `ski.jpg` and `snowboard.jpg`
- Got flag parts, as follows:

Flag Part 1: `QPLDOHUDGSI` (decoded from morse using `VH9rua2pwlkjf82W` as passphrase with `steghide` to get `flagpart1.wav`)
Flag Part 2: `sfgTYgcyfqewr3` (String at end of `snowboard.jpg`)
Flag Part 3: `dtfghJTs` (QR code in `broken.png`)

- Results in `QPLDOHUDGSIsfgTYgcyfqewr3dtfghJTs` when concatenated

```
QPLDOHUDGSIsfgTYgcyfqewr3dtfghJTs
```





