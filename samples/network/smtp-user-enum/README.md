# Smtp User Enum

## Scenario

10.10.1.66 enumerated mailbox users on the SMTP server 10.10.2.25 using VRFY, learning which accounts exist from 252 vs 550 replies.

Evidence: `smtp-user-enum.pcap`  
Capture date: 2019-07-07

## Timeline

~11:11  Repeated VRFY commands; the server answers 252 for real users and 550 for unknown ones.

## How to spot it

- Repeated SMTP VRFY (or EXPN/RCPT) commands
- 252 responses reveal valid users, 550 reject
- One client probing many usernames
- Cleartext on TCP 25

## Questions

```
1. Scanner?           10.10.1.66
2. Target?             10.10.2.25 (SMTP)
3. Command?            VRFY
4. Valid users?         ahmad, sara, root, omar
5. Technique?          SMTP user enumeration
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 0484dc9154cfaca3124ae7490aad6a747a3a2e69f95de6fab9e53f7e7565f694.zip -phidden`. You get `smtp-user-enum.pcap`.
2. Open `smtp-user-enum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `smtp`  (SMTP commands (VRFY, MAIL, RCPT)). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r smtp-user-enum.pcap -Y "smtp"`, and list talkers with `tshark -r smtp-user-enum.pcap -q -z conv,ip`.
5. Confirm against the answer: Repeated SMTP VRFY (or EXPN/RCPT) commands

## MITRE ATT&CK

- T1087.003 Email Account
- T1589.002 Email Addresses

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
