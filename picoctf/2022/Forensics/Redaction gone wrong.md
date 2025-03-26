# Redaction gone wrong

Now you DON’T see me. This [report](https://artifacts.picoctf.net/c/84/Financial_Report_for_ABC_Labs.pdf) has some critical data in it, some of which have been redacted correctly, while some were not. Can you find an important key that was not redacted properly?

-----

We are given a PDF file, `Financial_Report_for_ABC_Labs.pdf`, which has elements redacted:

![[picoctf/images/Pasted image 20250225141618.png]]

Copying the contents into notepad, we see that the redactions are simply black images overlaying text:

![[picoctf/images/Pasted image 20250225141742.png]]

```
picoCTF{C4n_Y0u_S33_m3_fully}
```