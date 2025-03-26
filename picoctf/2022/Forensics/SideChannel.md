# SideChannel

There's something fishy about this PIN-code checker, can you figure out the PIN and get the flag? Download the PIN checker program here [pin_checker](https://artifacts.picoctf.net/c/72/pin_checker)

Once you've figured out the PIN (and gotten the checker program to accept it), connect to the master server using `nc saturn.picoctf.net 58475` and provide it the PIN to get your flag.

-----

We are given an ELF binary:

```
$ file pin_checker
pin_checker: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, stripped
```

```
$ checksec --file=pin_checker
RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      Symbols         FORTIFY Fortified       Fortifiable     FILE
Partial RELRO   No canary found   NX disabled   No PIE          No RPATH   No RUNPATH   No Symbols        No    0               2               pin_checker
```

