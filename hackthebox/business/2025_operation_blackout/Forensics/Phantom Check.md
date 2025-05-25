# Phantom Check

Talion suspects that the threat actor carried out anti-virtualization checks to avoid detection in sandboxed environments. Your task is to analyze the event logs and identify the specific techniques used for virtualization detection. Byte Doctor requires evidence of the registry checks or processes the attacker executed to perform these checks.

-----

1. Which WMI class did the attacker use to retrieve model and manufacturer information for virtualization detection?

```
Win32_ComputerSystem
```

2. Which WMI query did the attacker execute to retrieve the current temperature value of the machine?

```
SELECT * FROM MSAcpi_ThermalZoneTemperature
```

3. The attacker loaded a PowerShell script to detect virtualization. What is the function name of the script?

```
Check-VM
```

4. The script enumerates the registry for virtualization services. Which key is being enumerated?

```
HKLM:\SYSTEM\ControlSet001\Services
```

5. When identifying the presence of VirtualBox, which two processes are being checked for existing? (`ServiceA.exe:ServiceB.exe`)

```
vboxservice.exe:vboxtray.exe
```

6. The VM detection script prints any detection with the prefix 'This is a'. Which two virtualization platforms did the script detect? (`ServiceA:ServiceB`)

```
ParameterBinding(Out-Default): name="InputObject"; value="This is a Hyper-V machine."
ParameterBinding(Out-Default): name="InputObject"; value="This is a VMWare machine."
```

```
Hyper-V:VMWare
```