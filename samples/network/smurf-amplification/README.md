# Smurf Amplification

## Scenario

An attacker sent ICMP echo requests with the victim's spoofed source address to the subnet broadcast, so every host floods the victim with replies, a smurf amplification attack.

Evidence: `smurf-amplification.pcap`  
Capture date: 2019-03-14

## Timeline

~12:03  Echo requests to the broadcast with a spoofed victim source; many hosts reply to the victim.

## How to spot it

- ICMP echo requests sent to a broadcast address
- Spoofed source equals the victim IP
- Many hosts reply to one victim (amplification)
- Victim drowns in echo replies

## Questions

```
1. Technique?         Smurf amplification
2. Trigger?            Echo to broadcast, spoofed src
3. Victim?             10.10.2.50
4. Amplifier?           all subnet hosts
5. Effect?             DoS by reply flood
```

## MITRE ATT&CK

- T1498.001 Direct Network Flood

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
