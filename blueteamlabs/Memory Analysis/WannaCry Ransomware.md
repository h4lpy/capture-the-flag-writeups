# Memory Analysis - Ransomware

The Account Executive called the SOC earlier and sounds very frustrated and angry. He stated he can’t access any files on his computer and keeps receiving a pop-up stating that his files have been encrypted. You disconnected the computer from the network and extracted the memory dump of his machine and started analyzing it with Volatility. Continue your investigation to uncover how the ransomware works and how to stop it!

-----

1. Run `vol.py -f infected.vmem --profile=Win7SP1x86 psscan` that will list all processes. What is the name of the suspicious process?

```
$ vol2.py -f infected.vmem --profile=Win7SP1x86 psscan
Offset(P)          Name                PID   PPID PDB        Time created                   Time exited                   
------------------ ---------------- ------ ------ ---------- ------------------------------ ------------------------------
...
0x000000001fcc6800 @WanaDecryptor     3968   2732 0x1e6d95c0 2021-01-31 18:02:48 UTC+0000
```

```
@WanaDecryptor
```

2. What is the parent process ID for the suspicious process?

```
2732
```

3. What is the initial malicious executable that created this process?

From the above `psscan`, PID `2732` corresponds to:

```
$ vol2.py -f infected.vmem --profile=Win7SP1x86 psscan
...
0x000000001fcd4350 or4qtckT.exe       2732   1456 0x1e6d94c0 2021-01-31 18:02:16 UTC+0000
```

```
or4qtckT.exe
```

4. If you drill down on the suspicious PID (`vol.py -f infected.vmem --profile=Win7SP1x86 psscan | grep (PIDhere)`), find the process used to delete files

```
$ vol2.py -f infected.vmem --profile=Win7SP1x86 psscan | grep 2732
...
0x000000001e992a88 taskdl.exe         4060   2732 0x1e6d9540 2021-01-31 18:24:54 UTC+0000   2021-01-31 18:24:54 UTC+0000  
0x000000001ef9ed40 @WanaDecryptor     2688   2732 0x1e6d9460 2021-01-31 18:24:49 UTC+0000   2021-01-31 18:24:49 UTC+0000  
0x000000001fcc6800 @WanaDecryptor     3968   2732 0x1e6d95c0 2021-01-31 18:02:48 UTC+0000                                 
0x000000001fcd4350 or4qtckT.exe       2732   1456 0x1e6d94c0 2021-01-31 18:02:16 UTC+0000 
```

```
taskdl.exe
```

5. Find the path where the malicious file was first executed

```
$ vol2.py -f infected.vmem --profile=Win7SP1x86 filescan | grep or4qtckT.exe
...
0x000000001ed75ae8      7      0 R--r-- \Device\HarddiskVolume1\Users\hacker\Desktop\or4qtckT.exe
0x000000001fcaf798      3      0 R--r-d \Device\HarddiskVolume1\Users\hacker\Desktop\or4qtckT.exe
```

```
C:\Users\hacker\Desktop\or4qtckT.exe
```

Alternatively:

```
$ vol2.py -f infected.vmem --profile=Win7SP1x86 cmdline --pid 2732
...
or4qtckT.exe pid:   2732
Command line : "C:\Users\hacker\Desktop\or4qtckT.exe" 
```

6. Can you identify what ransomware it is? (Do your research!)

```
Wannacry
```

7. What is the filename for the file with the ransomware public key that was used to encrypt the private key? (`.eky` extension)

```
$ vol2.py -f infected.vmem --profile=Win7SP1x86 filescan | grep ".eky"
...
0x000000001fca6268     11      1 -W-r-- \Device\HarddiskVolume1\Users\hacker\Desktop\00000000.eky
```