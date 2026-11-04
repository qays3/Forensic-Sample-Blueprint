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

## MITRE ATT&CK
- T1110.003 Brute Force: Password Spraying

## Tools
Real capture: impacket getTGT against a Samba AD DC, one attempt per account. tcpdump on the link.


## Signature
Forensic Sample Blueprint | qayssarayra.com
