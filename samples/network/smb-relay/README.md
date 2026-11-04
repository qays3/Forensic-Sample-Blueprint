# smb-relay

## Scenario
An attacker (10.10.1.66) relays an SMB authentication from a victim (10.10.1.50) to another server (10.10.2.22) where SMB signing is off, gaining access as the victim without cracking anything.

## Timeline (UTC+3)
```
10:10:00  Victim authenticates to attacker over SMB
10:10:00  Attacker relays the same NTLM auth to 10.10.2.22 (signing off)
```

## How to spot it
- The same NTLM authentication appears in two SMB sessions
- The attacker sits between victim and target
- SMB signing is not required on the target
- No password cracking needed, the auth is reused

## Questions
```
1. Victim?           10.10.1.50
2. Relay?            10.10.1.66
3. Target?           10.10.2.22
4. Why possible?     SMB signing off on the target
5. Technique?         SMB relay
```

## MITRE ATT&CK
- T1557.001 Adversary in the Middle
- T1021.002 SMB/Windows Admin Shares

## Tools
Crafted SMB relay (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
