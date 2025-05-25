# Smoke & Mirrors

Byte Doctor Reyes is investigating a stealthy post-breach attack where several expected security logs and Windows Defender alerts appear to be missing. He suspects the attacker employed defense evasion techniques to disable or manipulate security controls, significantly complicating detection efforts. Using the exported event logs, your objective is to uncover how the attacker compromised the system's defenses to remain undetected.



-----

1. The attacker disabled LSA protection on the compromised host by modifying a registry key. What is the full path of that registry key?

```
HKLM\SYSTEM\CurrentControlSet\Control\LSA
```

2. Which PowerShell cmdlet controls Windows Defender?

```
Set-MpPreference
```

3. The attacker loaded an AMSI patch written in PowerShell. Which function in the amsi.dll is being patched by the script to disable AMSI? Hint: The script in question imports 'kernel32.dll'

```
AmsiScanBuffer
```

4. Which command did the attacker use to restart the machine in Safe Mode, (with arguments, without ".exe")?

```
bcdedit /set safeboot network
```

5. Which PowerShell command did the attacker use to disable PowerShell command history logging?

```
Set-PSReadlineOption -HistorySaveStyle SaveNothing 
```

