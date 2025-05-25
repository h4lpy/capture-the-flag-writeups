# Ghost Thread

Byte Doctor suspects the attacker used a process injection technique to run malicious code within a legitimate process, leaving minimal traces on the file system. The logs reveal Win32 API calls that hint at a specific injection method used in the attack. Your task is to analyze these logs using a tool called API Monitor to uncover the injection technique and identify which legitimate process was targeted.

-----

1. What process injection technique is being used? Hint: T***** L**** S******

```
Thread Local Storage
```

2. Which Win32 API was used to take snapshots of all processes and threads on the system?

```
CreateToolhelp32Snapshot
```

3. Which process is the attacker's binary attempting to locate for payload injection?

```

```

4. What is the process ID of the identified process?

```

```

5. What is the size of the shellcode?

```

```

6. Which Win32 API was used to execute the injected payload in the identified process?

```

```

7. The injection method used by the attacker executes before the `main()` function is called. Which Win32 API is responsible for terminating the program before `main()` runs?