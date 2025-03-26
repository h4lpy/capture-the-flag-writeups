# TXT Message

Hmmm, have you seen some of the strange DNS records for the `ctf.games` domain? One of them sure is [od](https://en.wikipedia.org/wiki/Od_(Unix))d...

-----

Performing an `nslookup` against `ctf.games`, we see an unusual TXT record containing an encoded string

```
$ nslookup -type=TXT ctf.games
Server:         192.168.1.254
Address:        192.168.1.254#53

Non-authoritative answer:
ctf.games       text = "146 154 141 147 173 061 064 145 060 067 062 146 067 060 065 144 064 065 070 070 062 064 060 061 144 061 064 061 143 065 066 062 146 144 143 060 142 175"

Authoritative answers can be found from:
```

Using an [Octal to Text Converter](https://v2.cryptii.com/octal/text), we can get the flag:

```
146 154 141 147 173 061 064 145 060 067 062 146 067 060 065 144 064 065 070 070 062 064 060 061 144 061 064 061 143 065 066 062 146 144 143 060 142 175
```

![[huntress/2024/images/Pasted image 20241008125204.png]]

```
flag{14e072f705d45882401d141c562fdc0b}
```