# The Nexus Breach

In an era fraught with cyber threats, Talion "Byte Doctor" Reyes, a former digital forensics examiner for an international crime lab, has uncovered evidence of a breach targeting critical systems vital to national infrastructure. Subtle traces of malicious activity point to a covert operation orchestrated by the Empire of Volnaya, a nation notorious for its expertise in cyber sabotage and hybrid warfare tactics. The breach threatens to disrupt essential services that Task Force Phoenix relies on in its ongoing fight against the expansion of the Empire of Volnaya. The attackers have exploited vulnerabilities in interconnected systems, employing sophisticated techniques to evade detection and trigger widespread disruption Can you analyze the digital remnants left behind, reconstruct the attack timeline, and uncover the full extent of the threat?

-----

1. Which credentials has been used to login on the platform? (e.g. username:password)

Packet 906

```
admin:dL4zyVJ1y8UhT1hX1m
```

2. Which Nexus OSS version is in use? (e.g. 1.10.0-01)

Packet 988

```
2.15.1-02
```

3. The attacker created a new user for persistence. Which credentials has been set? (e.g. `username:password`)

Packet 920

```
adm1n1str4t0r:46vaGuj566
```

4. One core library written in Java has been tampered and replaced by a malicious one. Which is its package name? (e.g. com.company.name)

```
com.phoenix.toolkit
```

5. The tampered library contains encrypted communication logic. What is the secret key used for session encryption? (e.g. Secret123)

```

```

6. Which is the name of the function that manages the (AES) string decryption process? (e.g. aVf41)

```
uJtXq5
```

7. Which is the system command that triggered the reverse shell execution for this session running the tampered JAR? (e.g. "java .... &")

```
java -jar /sonatype-work/storage/snapshots/com/phoenix/toolkit/1.0/PhoenixCyberToolkit-1.0.jar &
```

8. Which other legit user has admin permissions on the Nexus instance (excluding "adm1n1str4t0r" and "admin")? (e.g. john_doe)

```

```

9. The attacker wrote something in a specific file to maintain persistence, which is the full path? (e.g. /path/file)

```

```

10. Which is the first executed command in the encrypted reverse shell session? (e.g. whoami)

```

```