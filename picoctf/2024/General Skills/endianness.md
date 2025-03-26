# endiannes

Know of little and big endian? [Source](https://artifacts.picoctf.net/c_titan/80/flag.c)

Additional details will be available after launching your challenge instance.

-----

Connecting to the remote instance, we get given a word, `xhsyw`, which we have to convert to both Little and Big Endian. To do this, we convert the string to hex with Little Endian being `7779736878` and Big Endian being `7868737977`.

Inputting these returns the flag:

```
Welcome to the Endian CTF!
You need to find both the little endian and big endian representations of a word.
If you get both correct, you will receive the flag.
Word: xhsyw
...
Enter the Little Endian representation: 7779736878
Correct Little Endian representation!
...
Enter the Big Endian representation: 7868737977
Correct Big Endian representation!
...
Congratulations! You found both endian representations correctly!
Your Flag is: picoCTF{3ndi4n_sw4p_su33ess_02999450}
```