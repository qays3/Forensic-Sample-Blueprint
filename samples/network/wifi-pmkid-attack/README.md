# wifi-pmkid-attack

## Scenario
An attacker captures the PMKID from the first EAPOL message of a WPA2 association. Unlike the four way handshake, a PMKID attack needs no client, just one frame from the AP, and can be cracked offline.

## Timeline (UTC+3)
```
Monitor mode capture
Association request to the AP
The AP sends EAPOL M1 containing the PMKID, captured for offline cracking
```

## How to spot it
- A single EAPOL message carrying a PMKID
- No full four way handshake or client needed
- Just one frame from the AP is enough
- PMKID is cracked offline like a handshake

## Questions
```
1. Access point?     00:11:22:33:44:55
2. What is captured?  PMKID (in EAPOL M1)
3. Client needed?    No, unlike the 4-way handshake
4. Frames needed?    One from the AP
5. Next step?        Offline crack
```

## MITRE ATT&CK
- T1040 Network Sniffing
- T1110.002 Brute Force: Password Cracking

## Tools
Crafted PMKID EAPOL frame (scapy, RadioTap). Wireless monitor mode capture.

## Signature
Forensic Sample Blueprint | qayssarayra.com
