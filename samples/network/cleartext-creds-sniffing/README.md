# cleartext-creds-sniffing

## Scenario
Several services on the network still use plaintext protocols. An attacker on the same segment (or on a tap or span port) can read the credentials straight off the wire. This capture holds FTP, HTTP basic auth, POP3 and Telnet logins in the clear.

## Timeline (UTC+3)
```
10:01:40  FTP login (accounting) in clear on 10.10.2.40
10:02:xx  HTTP basic auth (jdoe) base64 in Authorization header
10:03:xx  POP3 login (mscott) in clear
10:04:xx  Telnet login (svcbackup) in clear
```

## How to spot it
- Protocols FTP, HTTP basic, POP3 and Telnet send credentials unencrypted
- FTP shows USER and PASS commands in plain text
- HTTP basic auth is base64 in the Authorization header, trivially decoded
- No attacker packets are needed, sniffing is passive

## Questions
```
1. Which protocols leak creds?   FTP, HTTP basic, POP3, Telnet
2. FTP account and password?     accounting / Autumn2026!
3. HTTP basic creds (decoded)?    jdoe / Welcome1
4. Is the HTTP password hidden?   No, base64 is not encryption
5. Why no attacker IP?           The capture is passive sniffing
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 48c05d2c6400dd4e1120aaa7665e5df68334446646645897461eba099f0c2a59.zip -phidden`. You get `cleartext-creds-sniffing.pcap`.
2. Open `cleartext-creds-sniffing.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ftp || ftp-data || telnet`  (cleartext FTP/Telnet commands and credentials). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r cleartext-creds-sniffing.pcap -Y "ftp || ftp-data || telnet"`, and list talkers with `tshark -r cleartext-creds-sniffing.pcap -q -z conv,ip`.
5. Confirm against the answer: Protocols FTP, HTTP basic, POP3 and Telnet send credentials unencrypted

## MITRE ATT&CK
- T1040 Network Sniffing
- T1552.001 Unsecured Credentials: Credentials In Files

## Tools
Crafted plaintext sessions (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
