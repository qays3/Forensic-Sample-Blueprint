# ntlm-relay

## Scenario
An attacker (10.10.1.66) receives an NTLM authentication from a victim (10.10.1.50) and relays it, unchanged, to a third server (10.10.2.22) that does not require SMB signing. The same challenge appears in both connections, and the attacker gains access to the target as the victim.

## Timeline (UTC+3)
```
11:25:00  Victim 10.10.1.50 authenticates to attacker 10.10.1.66 over SMB (NTLMSSP)
11:25:00  Attacker opens SMB to 10.10.2.22 and replays the same NTLM challenge
11:25:xx  Auth succeeds on the target, attacker writes to ADMIN$
```

## How to spot it
- Two SMB sessions share the exact same NTLM server challenge
- The attacker sits between the victim and the target
- SMB signing is off on the target, which is what allows the relay
- After auth the attacker writes a service binary to ADMIN$

## Questions
```
1. Victim IP?           10.10.1.50
2. Relay (attacker)?    10.10.1.66
3. Final target?        10.10.2.22
4. What links the two sessions?   The same NTLM challenge
5. Why possible?        SMB signing not required on the target
```

## MITRE ATT&CK
- T1557.001 Adversary in the Middle
- T1021.002 Remote Services: SMB/Windows Admin Shares

## Tools
Crafted NTLMSSP relay across two SMB sessions (scapy) over generated background traffic


## Note
This pcap is a representative sample built with scapy: ports and markers are correct but the payload bytes are placeholders, not full protocol data. A faithful capture of this attack needs a real Windows domain controller, which this lab (Samba) cannot reproduce.

## Signature
Forensic Sample Blueprint | qayssarayra.com
