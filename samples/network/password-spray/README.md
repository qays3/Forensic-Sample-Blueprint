# password-spray

## Scenario
Real capture against a live Active Directory domain controller (Samba AD, corp.local, DC 10.10.2.10). An attacker at 10.10.1.66 tries a single password against many accounts through Kerberos pre-authentication. Most fail, one succeeds. This is genuine traffic, not crafted.

## Timeline (UTC+3)
```
Attacker 10.10.1.66 sends AS-REQ with one password for each of 6 accounts to DC 10.10.2.10
Five accounts return KDC_ERR_PREAUTH_FAILED
One account (dave) returns a TGT, the spray hit
```

## How to spot it
- One source sends AS-REQ pre-auth attempts for many usernames
- One attempt per account, the same password, spaced out to dodge lockout
- Most replies are pre-auth failures, one is a success
- On the DC this is many 4771 / 4768 events from one host

## Questions
```
1. Attacker IP?          10.10.1.66
2. Domain controller?    10.10.2.10 (corp.local)
3. Pattern?              One password, many users (spray)
4. Accounts tried?       alice, bob, carol, dave, erin, administrator
5. Which succeeded?      dave
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2eeb423bbd4d861ba948a6ce41c3b6db87d8763676eb918a7f4dcf8f6255c465.zip -phidden`. You get `password-spray.pcap`.
2. Open `password-spray.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r password-spray.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r password-spray.pcap -q -z conv,ip`.
5. Confirm against the answer: One source sends AS-REQ pre-auth attempts for many usernames

## MITRE ATT&CK
- T1110.003 Brute Force: Password Spraying

## Tools
Real capture: impacket getTGT against a Samba AD DC, one attempt per account. tcpdump on the link.


## Signature
Forensic Sample Blueprint | qayssarayra.com
