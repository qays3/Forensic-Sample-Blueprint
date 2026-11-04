# rdp-brute-force

## Scenario
An external host (188.92.77.14) ran an RDP brute force against the server 10.10.2.30. Each try is a short connection carrying a different mstshash cookie (the username), and one final long session is the successful login.

## Timeline (UTC+3)
```
11:03:20  RDP brute force starts from 188.92.77.14 to 10.10.2.30 port 3389
11:0x:xx  ~180 short connections, each with a mstshash cookie, reset fast
later     One long session, the successful login
```

## How to spot it
- One external IP opens many connections to port 3389
- Each connection request holds a Cookie: mstshash=<user>, the tried username
- Failed tries reset quickly, the success stays open
- External source to internal RDP is itself unusual

## Questions
```
1. Attacker IP?          188.92.77.14
2. Target and port?      10.10.2.30 port 3389
3. What field leaks the username?   mstshash cookie
4. Usernames tried?      administrator, admin, user, backup, sql, helpdesk
5. Which try succeeded?  The long final session (administrator)
```

## MITRE ATT&CK
- T1110.001 Brute Force: Password Guessing
- T1021.001 Remote Services: Remote Desktop Protocol

## Tools
Crafted RDP connection requests (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
