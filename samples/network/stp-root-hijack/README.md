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

## MITRE ATT&CK
- T1557 Adversary in the Middle
- T1498 Network Denial of Service

## Tools
Crafted STP BPDU hijack (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
