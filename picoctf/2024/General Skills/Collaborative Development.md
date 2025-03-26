# Collaborative Development

My team has been working very hard on new features for our flag printing program! I wonder how they'll work together? You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/176/challenge.zip)

-----

Unzipping the provided archive, we are given a `flag.py` file which contains the following code:

```python
print("Printing the flag...")
```

We are also given a `.git` directory which we can check the commit history of with `git log`:

```
$ git log
commit eb19d0e3c28278752f0735c4451b885136a24105 (HEAD -> main)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:49 2024 +0000

    init flag printer
```

This does not reveal anything interesting. Pivoting to the available branches, we see there are four available:

```
$ git branch -a
* (HEAD detached at eb19d0e)
  feature/part-1
  feature/part-2
  feature/part-3
  main
```

Switching to each `feature/` branch with `git switch`, we get the contents of the flag:

```
$ git switch feature/part-1
$ cat flag.py
print("Printing the flag...")
print("picoCTF{t3@mw0rk_", end='')%   
...
$ git switch feature/part-2
$ cat flag.py
print("Printing the flag...")
print("m@k3s_th3_dr3@m_", end='')%  
...
$ git switch feature/part-3
$ cat flag.py
print("Printing the flag...")
print("w0rk_2c91ca76}")
```


```
picoCTF{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_2c91ca76}
```