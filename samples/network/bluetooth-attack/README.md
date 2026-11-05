# Bluetooth Attack

## Scenario

A Bluetooth reconnaissance and exploitation capture: an inquiry scan discovering nearby devices followed by an SDP overflow attempt (BlueBorne style) against a headset.

Evidence: `bluetooth-attack.pcap`  
Capture date: 2019-02-16

## Timeline

~13:08  Inquiry scan enumerates BD_ADDRs and device names, then an SDP overflow attempt.

## How to spot it

- Bluetooth inquiry scan listing BD_ADDRs and names
- Device classes and names leaked
- An SDP/L2CAP overflow attempt
- Targets a nearby peripheral

## Questions

```
1. Phase 1?           Inquiry scan (discovery)
2. Leaked?             BD_ADDR, device name/class
3. Phase 2?            SDP overflow (BlueBorne-style)
4. Target?             CorpHeadset
5. Technique?          Bluetooth attack
```

## MITRE ATT&CK

- T1011 Exfiltration Over Other Network Medium

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
