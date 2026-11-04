# llmnr-nbtns-poisoning

## Scenario
A workstation (10.10.1.50) asks for hostnames that DNS could not resolve, so Windows falls back to LLMNR and NBT-NS. An attacker running Responder (10.10.1.66) answers, claiming to be those hosts. The victim then connects to the attacker over SMB and sends its NTLM authentication, so the attacker captures the hash.

## Timeline (UTC+3)
```
13:15:00  Victim 10.10.1.50 sends LLMNR query for fileshrv (multicast 224.0.0.252)
13:15:00  Attacker 10.10.1.66 answers, mapping the name to itself
13:15:xx  Same for hr-share and printsvr01, plus NBT-NS broadcasts
13:15:xx  Victim opens SMB to 10.10.1.66 and sends NTLMSSP auth (hash captured)
```

## How to spot it
- Queries to LLMNR (UDP 5355, multicast 224.0.0.252) and NBT-NS (UDP 137 broadcast)
- One host answers every name resolution, pointing them all at itself
- The victim then authenticates to that host over SMB with NTLMSSP
- The attacker IP is internal, the hallmark of Responder on the LAN

## Questions
```
1. Victim IP?           10.10.1.50
2. Attacker (Responder)?  10.10.1.66
3. Protocols abused?    LLMNR (5355) and NBT-NS (137)
4. What does the attacker get?  The victim NTLM hash over SMB
5. Account that leaked?  jsmith
```

## MITRE ATT&CK
- T1557.001 Adversary in the Middle: LLMNR/NBT-NS Poisoning and SMB Relay
- T1040 Network Sniffing

## Tools
Crafted LLMNR, NBT-NS and NTLMSSP exchange (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
