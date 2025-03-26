# Challenge 03 | Bonus

## Overview

Who called anymore? +1 833-269-5744

Hint: Listen for the tone. This one may be a little harder. Head over to the Discord if you need help.

## Walkthrough

When calling the number, we are presented with an AI voice which indicates we must navigate through a maze using our keypad to get to the flag.

Within [EP000: Operation Aurora](https://youtu.be/przDcQe6n5o?t=715), there is a maze animation which is identical to the maze from the phonecall.  Using the keypad, we derive the following sequence:

```
88888888888888668888666688668844444444224488886666666666668866888888666622
```

Note, `2`=up, `4`=left, `6`=right, and `8`=down.

When we enter this sequence, we get the flag:

Flag: `https://h4ck1ng.google/solve/up_up_down_down_left_right_left_right`