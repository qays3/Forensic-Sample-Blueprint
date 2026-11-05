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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 83add6f0ded2c1e4587f2b53bf70c2f94397dfc222d7f4a73bcb139736684492.zip -phidden`. You get `telnet-ftp-cleartext.pcap`.
2. Open `telnet-ftp-cleartext.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ftp || ftp-data || telnet`  (cleartext FTP/Telnet commands and credentials). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r telnet-ftp-cleartext.pcap -Y "ftp || ftp-data || telnet"`, and list talkers with `tshark -r telnet-ftp-cleartext.pcap -q -z conv,ip`.
5. Confirm against the answer: USER and PASS visible in plaintext

## MITRE ATT&CK

- T1040 Network Sniffing

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
