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

## MITRE ATT&CK

- T1087.003 Email Account
- T1589.002 Email Addresses

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
