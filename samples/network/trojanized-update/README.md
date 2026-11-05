# Trojanized Update

## Scenario

A software update is trojanized at the source: the client pulls an update package from the vendor update host, then the installed backdoor beacons to an attacker endpoint.

Evidence: `trojanized-update.pcap`  
Capture date: 2023-06-04

## Timeline

~10:01  An update package pulled from the vendor host, then a beacon to an unrelated endpoint.

## How to spot it

- An update fetched from a normal vendor update host
- A beacon to an unrelated endpoint after update
- The update channel itself is the delivery vector
- Trusted software supply chain abused

## Questions

```
1. Technique?         Trojanized update
2. Update host?         updates.software-vendor.example
3. Beacon?             ops-telemetry.example
4. Vector?             supply chain
5. ATT&CK?             T1195.002
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `trojanized-update.pcap`.
2. Open `trojanized-update.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (the update fetch and the beacon). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r trojanized-update.pcap -Y "http || tls"`, and `tshark -r trojanized-update.pcap -q -z conv,ip`.
5. Confirm against the answer: An update fetched from a normal vendor update host

## MITRE ATT&CK

- T1195.002 Compromise Software Supply Chain

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
