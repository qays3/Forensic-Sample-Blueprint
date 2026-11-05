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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4acae2293490b1c7070487c42e7c7aea31649fb712475dd8499095f450f02f28.zip -phidden`. You get `ftp-exfil.pcap`.
2. Open `ftp-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ftp || ftp-data || telnet`  (cleartext FTP/Telnet commands and credentials). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ftp-exfil.pcap -Y "ftp || ftp-data || telnet"`, and list talkers with `tshark -r ftp-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: Outbound FTP to an external server, unusual for a workstation

## MITRE ATT&CK
- T1048.003 Exfiltration Over Unencrypted Non-C2 Protocol

## Tools
Crafted FTP exfil (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
