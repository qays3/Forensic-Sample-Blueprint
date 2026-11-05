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

## MITRE ATT&CK

- T1557 Adversary-in-the-Middle

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
