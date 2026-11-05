# Watering Hole

## Scenario

A watering hole attack: an industry site that the target group visits has been compromised to serve a profiling and payload script to its visitors.

Evidence: `watering-hole.pcap`  
Capture date: 2024-09-01

## Timeline

~11:11  Several internal hosts visit the same legit industry site and all receive an injected script.

## How to spot it

- Multiple internal hosts visiting the same legit site
- An injected script served to each visitor
- The site is relevant to the target's sector
- Compromise of a trusted third party site

## Questions

```
1. Technique?         Watering hole
2. Site?               sector-association.example
3. Injected?           /inject.js
4. Victims?            several internal hosts
5. ATT&CK?             T1189
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `watering-hole.pcap`.
2. Open `watering-hole.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (the injected script requests). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r watering-hole.pcap -Y "http || tls"`, and `tshark -r watering-hole.pcap -q -z conv,ip`.
5. Confirm against the answer: Multiple internal hosts visiting the same legit site

## MITRE ATT&CK

- T1189 Drive-by Compromise
- T1608.004 Drive-by Target

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
