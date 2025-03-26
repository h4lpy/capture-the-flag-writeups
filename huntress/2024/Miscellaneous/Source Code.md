# Source Code

Follow the white rabbit.

-----

Navigating to the website, we are presented with a matrix-like site with a single input box.

![[huntress/2024/images/Pasted image 20241008122213.png]]

Inputting a test string results in the following error code:

![[huntress/2024/images/Pasted image 20241008122253.png]]

Looking at the source code, we see that [Rezmason's matrix](https://github.com/Rezmason/matrix) is being used to render the glyphs. There is also a `<script>` tag at the bottom which confirms `enter` uses the input value we provide to move to the specified endpoint:

```js
try {
	const response = await fetch(`/enter=${inputValue.trim()}`, {
	  method: "GET",
});
```

Referencing the [original matrix repo](https://github.com/Rezmason/matrix), I saw that the site's `js/config.js` contained an additional line:

```js
backupGlyphsTwr: ["a", "b", "c", "d", "e", "f"], // The characters to fallback to if glyphs fail to load
```

Assuming `Twr` meant **The White Rabbit**, I concluded that `["a", "b", "c", "d", "e", "f"]` must be the character set used for the password to get the flag.

As bruteforcing was allowed for this challenge, I crafted a wordlist with `crunch` using these characters and ran `ffuf`, filtering out any responses containing "Incorrect":

```console
# Creates a wordlist using "abcdef" as character set from lengths 1-8 (a to ffffffff), outputting to wordlist.txt
$ crunch 1 8 abcdef -o wordlist.txt

# Fuzz '/enter=' using the wordlist, filtering out any responses with "Incorrect"
$ ffuf -u http://challenge.ctf.games:[port]/enter=FUZZ -w wordlist.txt -fr "Incorrect"
```

This results in a `200 OK` for the string `bfdaec` which returns the flag:

![[huntress/2024/images/Pasted image 20241008123323.png]]

![[huntress/2024/images/Pasted image 20241008121230.png]]

```
flag{dc9edf4624504202eec5d3fab10bbccd}
```