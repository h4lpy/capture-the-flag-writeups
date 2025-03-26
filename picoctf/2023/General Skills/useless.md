# useless

There's an interesting script in the user's home directory

-----

Connecting to the instance via SSH, we see a bash script named `useless`:

```bash
#!/bin/bash
# Basic mathematical operations via command-line arguments

if [ $# != 3 ]
then
  echo "Read the code first"
else
        if [[ "$1" == "add" ]]
        then
          sum=$(( $2 + $3 ))
          echo "The Sum is: $sum"

        elif [[ "$1" == "sub" ]]
        then
          sub=$(( $2 - $3 ))
          echo "The Substract is: $sub"

        elif [[ "$1" == "div" ]]
        then
          div=$(( $2 / $3 ))
          echo "The quotient is: $div"

        elif [[ "$1" == "mul" ]]
        then
          mul=$(( $2 * $3 ))
          echo "The product is: $mul"

        else
          echo "Read the manual"

        fi
fi
```

After the if-statements, there is an output to "read the manual". We can read the manual with `man useless` which contains the flag:

```
$ man useless

useless
     useless, — This is a simple calculator script

SYNOPSIS
     useless, [add sub mul div] number1 number2

DESCRIPTION
     Use the useless, macro to make simple calulations like addition,subtraction, multiplication and division.
...
picoCTF{us3l3ss_ch4ll3ng3_3xpl0it3d_4151}
```

```
picoCTF{us3l3ss_ch4ll3ng3_3xpl0it3d_4151}
```