# Hsrp Hijack

## Scenario

A host sent HSRP hello packets with the maximum priority and a coup state to take over the virtual gateway IP, so it can man-in-the-middle the subnet's traffic.

Evidence: `hsrp-hijack.pcap`  
Capture date: 2025-06-21

## Timeline

~12:00  HSRP packets from 10.10.1.66 with priority 255 claiming the active role for the gateway VIP.

## How to spot it

- HSRP hellos to 224.0.0.2 UDP 1985
- Priority 255 (max) from an unexpected host
- Coup/active state for the gateway VIP
- Default HSRP auth (cleartext cisco)

## Questions

```
1. Protocol?          HSRP (UDP 1985)
2. Attacker?           10.10.1.66
3. Priority?            255 (takes active)
4. VIP?                10.10.1.1 (gateway)
5. Technique?          First-hop redundancy hijack
```

## MITRE ATT&CK

- T1557 Adversary-in-the-Middle

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
