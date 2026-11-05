# Newly Registered Domain C2

## Scenario

A host beacons to a newly registered, low reputation domain: a DNS lookup for an odd .top domain, then regular HTTP check ins.

Evidence: `newly-registered-domain-c2.pcap`  
Capture date: 2022-02-13

## Timeline

~11:00  DNS for a strange new domain, then steady HTTP beacons to it.

## How to spot it

- DNS for a newly registered, random looking domain
- An unusual TLD (.top, .xyz)
- Regular HTTP check ins after resolution
- Low reputation destination

## Questions

```
1. Domain?            update-service-7f2a.top
2. Tell?               newly registered, odd TLD
3. Pattern?            steady beacons
4. Role?               C2
5. ATT&CK?             T1071.001
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `newly-registered-domain-c2.pcap`.
2. Open `newly-registered-domain-c2.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns || http`  (the DNS lookup and beacons). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r newly-registered-domain-c2.pcap -Y "dns || http"`, and `tshark -r newly-registered-domain-c2.pcap -q -z conv,ip`.
5. Confirm against the answer: DNS for a newly registered, random looking domain

## MITRE ATT&CK

- T1071.001 Web Protocols
- T1568 Dynamic Resolution

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
