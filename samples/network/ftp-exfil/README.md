# ftp-exfil

## Scenario
A host (10.10.1.56) uploads a database archive to an external FTP server (193.106.191.20) with a STOR command. The control channel is plaintext, so the filename customers_db.zip is visible.

## Timeline (UTC+3)
```
13:10:00  FTP login to 193.106.191.20, then STOR customers_db.zip
13:10:xx  Data channel transfers the archive out
```

## How to spot it
- Outbound FTP to an external server, unusual for a workstation
- STOR command with a sensitive filename in cleartext
- A data channel carrying bulk outbound bytes
- Plaintext FTP exposes the whole transfer

## Questions
```
1. Host?             10.10.1.56
2. FTP server?       193.106.191.20
3. Command?          STOR (upload)
4. File stolen?      customers_db.zip
5. Visible why?      FTP control channel is plaintext
```

## MITRE ATT&CK
- T1048.003 Exfiltration Over Unencrypted Non-C2 Protocol

## Tools
Crafted FTP exfil (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
