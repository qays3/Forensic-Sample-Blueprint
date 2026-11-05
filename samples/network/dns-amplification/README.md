# Dns Amplification

## Scenario

A DNS amplification attack: small ANY queries are sent to open resolvers with the victim's spoofed source, so large replies flood the victim.

Evidence: `dns-amplification.pcap`  
Capture date: 2020-06-23

## Timeline

~11:03  Small ANY queries to public resolvers spoofed as the victim, large replies sent to the victim.

## How to spot it

- ANY queries to open resolvers
- Source spoofed as the victim
- Replies much larger than the queries
- Victim floods with DNS responses it never asked for

## Questions

```
1. Technique?         DNS amplification
2. Victim?             203.0.113.5
3. Query?              ANY (small)
4. Reply?              large (amplified)
5. Effect?             reflected flood
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `dns-amplification.pcap`.
2. Open `dns-amplification.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (the amplified replies). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r dns-amplification.pcap -Y "dns"`, and `tshark -r dns-amplification.pcap -q -z conv,ip`.
5. Confirm against the answer: ANY queries to open resolvers

## MITRE ATT&CK

- T1498.002 Reflection Amplification

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
