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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x c2e1be56d70b2a0235bc09b48c7ef592c384147c3784ed660ecfab7bf8849fe2.zip -phidden`. You get `hsrp-hijack.pcap`.
2. Open `hsrp-hijack.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `hsrp`  (HSRP hellos on udp.port==1985). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r hsrp-hijack.pcap -Y "hsrp"`, and list talkers with `tshark -r hsrp-hijack.pcap -q -z conv,ip`.
5. Confirm against the answer: HSRP hellos to 224.0.0.2 UDP 1985

## MITRE ATT&CK

- T1557 Adversary-in-the-Middle

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
