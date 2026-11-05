# Bgp Hijack

## Scenario

A BGP speaker announced more specific prefixes it does not own (including a chunk of 8.8.8.0/24), a prefix hijack that pulls other networks' traffic toward the attacker.

Evidence: `bgp-hijack.pcap`  
Capture date: 2023-01-01

## Timeline

~13:00  TCP 179 session, OPEN, then UPDATE messages announcing prefixes the peer should not originate.

## How to spot it

- BGP UPDATEs announcing prefixes not owned
- More specific routes overriding legitimate ones
- An unexpected AS in the path
- TCP 179 session from an unusual peer

## Questions

```
1. Protocol?          BGP (TCP 179)
2. Announced?           8.8.8.0/24, 192.0.2.0/25
3. Problem?            Prefixes not owned (more specific)
4. Effect?             Traffic pulled to attacker
5. Technique?          BGP prefix hijack
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 9e5f37e721406157112bb98bd90420c6f2928275bf99f1c4c0b68a8d8e4f5fa7.zip -phidden`. You get `bgp-hijack.pcap`.
2. Open `bgp-hijack.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `bgp`  (BGP OPEN and UPDATE on tcp.port==179). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r bgp-hijack.pcap -Y "bgp"`, and list talkers with `tshark -r bgp-hijack.pcap -q -z conv,ip`.
5. Confirm against the answer: BGP UPDATEs announcing prefixes not owned

## MITRE ATT&CK

- T1557 Adversary-in-the-Middle

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
