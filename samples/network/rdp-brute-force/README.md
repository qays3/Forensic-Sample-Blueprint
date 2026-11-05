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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 85314c67dcb1a42c775a5c88ae93e212cec6d69909b9d55c51cec90aef71d3f1.zip -phidden`. You get `rdp-brute-force.pcap`.
2. Open `rdp-brute-force.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r rdp-brute-force.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r rdp-brute-force.pcap -q -z conv,ip`.
5. Confirm against the answer: One external IP opens many connections to port 3389

## MITRE ATT&CK
- T1110.001 Brute Force: Password Guessing
- T1021.001 Remote Services: Remote Desktop Protocol

## Tools
Crafted RDP connection requests (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
