# No need for Brutus

A simple message for you to decipher:  
  
`squiqhyiiycfbudeduutvehrhkjki`  
  
Submit the original plaintext hashed with MD5, wrapped between the usual flag format: `flag{}`

-----

Using [CyberChef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,10)MD5()&input=c3F1aXFoeWlpeWNmYnVkZWR1dXR2ZWhyaGtqa2kg), we see this is a Caesar cipher with a shift value of 10. The MD5 of the resulting string, `caesarissimplenoneedforbrutus`, is `c945bb2173e7da5a292527bbbc825d3f`.

```
flag{c945bb2173e7da5a292527bbbc825d3f}
```
