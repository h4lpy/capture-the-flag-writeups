# \[Medium] SOC 101 - Phishing Mail Detected

### Alert Details

Event ID: `87`
Event Time: `2021-04-04 23:00:00 UTC`
Rule: `SOC101 - Phishing Mail Detected`
Level: `Security Analyst`
SMTP Address: `146[.]56[.]195[.]192`
Source Address: `lethuyan852[@]gmail[.]com`
Destination Address: `markp[@]letsdefend[.]io`
E-mail Subject: `Its a Must have for your Phone`
Device Action: `Allowed`

### Initial Triage

- Received alert for a phishing mail being successfully delivered to recipient `markp[@]letsdefend[.]io`
- Sender is an external Google account user - `lethuyan852[@]gmail[.]com`
- Scoping for the sender reveals the email in question:
	- Contains a link to `.xyz` domain

```
Check out this product! Your life will be less difficult. hxxp[://]nuangaybantiep[.]xyz
```

### Email Analysis

- Checking the domain within [VirusTotal](https://www.virustotal.com/gui/search/nuangaybantiep.xyz) shows 1 malicious verdict and 1 suspicious verdict out of 90 vendors
- As per Whois, the domain is registered in Reykjavik under Namecheap hosting
- No attachments present