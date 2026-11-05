# stp-root-hijack

## Scenario
An attacker (10.10.1.66) sends spanning tree (STP) BPDUs claiming a superior (lower) bridge priority, forcing an election that makes the attacker the root bridge so traffic reroutes through it.

## Timeline (UTC+3)
```
09:55:00  Attacker emits BPDUs with a very low bridge priority
09:55:xx  STP topology recalculates, attacker becomes root
```

## How to spot it
- BPDU frames from an unexpected host (dst 01:80:c2:00:00:00)
- A new bridge claims root with a very low priority
- STP topology changes and traffic reroutes
- End hosts should not send BPDUs at all

## Questions
```
1. Attacker?         10.10.1.66
2. Protocol?         Spanning Tree (STP) BPDUs
3. Claim?            Superior (low) bridge priority
4. Result?           Attacker becomes root bridge
5. Red flag?         An end host sending BPDUs
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 095579c19b7efaed13a6beaba5b6e6b836cc1fbeb14a753b5ec626f07789acbd.zip -phidden`. You get `stp-root-hijack.pcap`.
2. Open `stp-root-hijack.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r stp-root-hijack.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r stp-root-hijack.pcap -q -z conv,ip`.
5. Confirm against the answer: BPDU frames from an unexpected host (dst 01:80:c2:00:00:00)

## MITRE ATT&CK
- T1557 Adversary in the Middle
- T1498 Network Denial of Service

## Tools
Crafted STP BPDU hijack (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
