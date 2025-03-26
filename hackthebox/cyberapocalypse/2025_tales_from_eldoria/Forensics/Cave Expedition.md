# Cave Expedition

Rumors of a black drake terrorizing the fields of Dunlorn have spread far and wide. The village has offered a hefty bounty for its defeat. Sir Alaric and Thorin answered the call also returning with treasures from its lair. Among the retrieved items they found a map. Unfortunately it cannot be used directly because a custom encryption algorithm was probably used. Luckily it was possible to retrieve the original code that managed the encryption process. Can you investigate about what happened and retrieve the map content?

-----

We are given two files:

- `Logs.zip`: Archive of Windows event logs
- `map.pdf.secured`: An encrypted PDF file containing the map we need to recover

Opening the `Logs.zip` archive, we see only the Sysmon logs contain data (`Microsoft-Windows-Sysmon_Operational.evtx`) spanning `2025-01-28 10:31:13` to `2025-01-28 10:31:25`:

![[Pasted image 20250322155057.png]]

Inspecting these logs, we see a large amount of `wevtutil.exe` which can be excluded, reducing the logs to only 25 events.

Initially, at `2025-01-28 10:31:17` there is a `cmd.exe` process executing a `avAFGrw41.bat` within `C:\Users\developer56546756\Desktop`:

![[Pasted image 20250322155435.png]]

Directly following this, there are 10 PowerShell commands which append segmented Base64 strings to a PowerShell script `avAFGrw41.ps1`.

![[Pasted image 20250322155841.png]]

This is followed by use of `certutil` to decode its contents before being executed at  `2025-01-28 10:31:19`.

Extracting each of the strings and decoding it from Base64 results in the completed script:

![[Pasted image 20250322160103.png]]

Reviewing the code, we can see the functionality is as follows:

1. Check user is `developer56546756` and hostname is `Workstation5678`
2. Check existence of path `dc01aq2/`
3. Loop through each file, XORing the contents using two encoded keys
4. Encode the file as Base64 and output it appending `.secured` extension

As we have access to both keys used for the XOR, we can leverage the existing functionality to decrypt the `map.pdf.secured` file:

```powershell
# Original encryption keys
$key1 = "NXhzR09iakhRaVBBR2R6TGdCRWVJOHUwWVNKcTc2RWl5dWY4d0FSUzdxYnRQNG50UVk1MHlIOGR6S1plQ0FzWg=="
$key2 = "n2mmXaWy5pL4kpNWr7bcgEKxMeUx50MJ"

# Decode function
function decodeFromBase64 {
    param([string]$encoded)
    return [Text.Encoding]::UTF8.GetString([Convert]::FromBase64String($encoded))
}

# Decode the first key (second key is already in plaintext)
$decodedKey1 = decodeFromBase64 $key1
$decodedKey2 = decodeFromBase64 $key2

# XOR function (same as in the original script)
function xorFunc {
    param([byte[]]$byteArr, [byte[]]$key1Bytes, [byte[]]$key2Bytes)
    $newByteArr = [byte[]]::new($byteArr.Length)
    for ($x = 0; $x -lt $byteArr.Length; $x++) {
        $xorByteKey1 = $key1Bytes[$x % $key1Bytes.Length]
        $xorByteKey2 = $key2Bytes[$x % $key2Bytes.Length]
        $newByteArr[$x] = $byteArr[$x] -bxor $xorByteKey1 -bxor $xorByteKey2
    }
    return $newByteArr
}

# Decrypt function
function decryptFile {
    param([string]$encryptedFilePath, [string]$outputFilePath)
    
    Write-Host "Reading encrypted file: $encryptedFilePath"
    
    try {
        # Read the encrypted content
        $encryptedBase64 = Get-Content $encryptedFilePath -Raw
        
        # Convert from Base64 to bytes
        $encryptedBytes = [Convert]::FromBase64String($encryptedBase64)
        
        Write-Host "Encrypted data size: $($encryptedBytes.Length) bytes"
        
        # Get the keys as bytes
        $key1Bytes = [System.Text.Encoding]::UTF8.GetBytes($decodedKey1)
        $key2Bytes = [System.Text.Encoding]::UTF8.GetBytes($decodedKey2)
        
        Write-Host "Decrypting using keys: $decodedKey1 and $decodedKey2"
        
        # Apply the XOR operation to decrypt
        $decryptedBytes = xorFunc $encryptedBytes $key1Bytes $key2Bytes
        
        # Write the decrypted content to the output file
        [IO.File]::WriteAllBytes($outputFilePath, $decryptedBytes)
        
        Write-Host "File successfully decrypted to: $outputFilePath"
    }
    catch {
        Write-Host "Error: $_"
    }
}

# Paths for the encrypted and output files
$encryptedFilePath = "map.pdf.secured"
$outputFilePath = "C:\Users\liamh\Documents\map.pdf"

# Run the decryption
Write-Host "Starting decryption process..."
decryptFile -encryptedFilePath $encryptedFilePath -outputFilePath $outputFilePath
Write-Host "Decryption process completed."
```

This results in the original `map.pdf` file with the flag:

![[Pasted image 20250322160617.png]]

```
HTB{Dunl0rn_dRAk3_LA1r_15_n0W_5AF3}
```