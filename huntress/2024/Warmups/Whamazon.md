# Whamazon

Wham! Bam! Amazon is entering the hacking business! Can you buy a flag?

-----

We are given a web instance to connect to which displays a menu comprising three options:

![[huntress/2024/images/Pasted image 20241008143950.png]]

Selecting `1` does not return anything useful. Choosing `2` will let you buy from the store from the initial 50 dollar you are given:

![[huntress/2024/images/Pasted image 20241008144035.png]]

Attempting to buy one Apple shows only numbers are accepted:

![[huntress/2024/images/Pasted image 20241008144856.png]]

Trying to do the same for the flag shows that it costs `10000000000` dollars.

![[huntress/2024/images/Pasted image 20241008145049.png]]

However, there is no checks to validate if the inputted number is positive, so we can provide a negative number to give us more dollars:

![[huntress/2024/images/Pasted image 20241008144950.png]]

From there, we can buy the flag:

![[huntress/2024/images/Pasted image 20241008145147.png]]

After a game of rock-paper-scissors, we get the flag added to our inventory:

![[huntress/2024/images/Pasted image 20241008145220.png]]

```
flag{18bdd83cee5690321bb14c70465d3408}
```