# Telnet Ftp Cleartext

## Scenario

FTP credentials cross the wire in cleartext: the USER and PASS commands, then a file retrieval, all readable by anyone capturing the segment.

Evidence: `telnet-ftp-cleartext.pcap`  
Capture date: 2021-05-13

## Timeline

~11:06  An FTP login with USER/PASS in plaintext followed by a RETR of a database backup.

## How to spot it

- USER and PASS visible in plaintext
- FTP control channel on TCP 21
- Credentials and filenames fully readable
- No TLS (not FTPS)

## Questions

```
1. Protocol?          FTP (TCP 21)
2. Creds?              admin / Pr0dFtp!2024 (cleartext)
3. Action?             RETR backup.sql
4. Risk?               Credential capture
5. Technique?          Cleartext protocol abuse
```

## MITRE ATT&CK

- T1040 Network Sniffing

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
