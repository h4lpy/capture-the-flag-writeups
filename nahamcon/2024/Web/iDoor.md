# iDoor

It's Apple's latest innovation, the "iDoor!" ... well, it is basically the Ring Doorbell camera, but the iDoor offers a web-based browser to monitor your camera, and super secure using ultimate cryptography with even SHA256 hashing algorithms to protect customers! Don't even _think_ about snooping on other people's cameras!!

-----

Web interface shows the following with SHA-256 in the URL:

![[Pasted image 20240524105610.png]]

`4fc82b26aecb47d2868c4efbe3581732a3e7cbcc6c2efb32062c08170a05eeb8` is the SHA-256 for `11` which we see is our `Customer ID`.

So we can compute the SHA-256 of other numbers, for example `1` =  `6B86B273FF34FCE19D6B804EFF5A3F5747ADA4EAA22F1D49C01E52DDB7875B4B`

![[Pasted image 20240524105934.png]]

The SHA-256 of `1` = `5feceb66ffc86f38d952786c6d696c79c2dbc239dd4e91b46729d73a27fb57e9`

![[Pasted image 20240524110001.png]]

```
flag{770a058a80a9bca0a87c3e2ebe1ee9b2}
```