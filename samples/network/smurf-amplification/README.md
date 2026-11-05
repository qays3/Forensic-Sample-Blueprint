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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 60b9cdee5dc98402c1e2c0867ba0e530c0f10f759078822a21c5ee4492b37b8c.zip -phidden`. You get `smurf-amplification.pcap`.
2. Open `smurf-amplification.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp`  (ICMP echo traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r smurf-amplification.pcap -Y "icmp"`, and list talkers with `tshark -r smurf-amplification.pcap -q -z conv,ip`.
5. Confirm against the answer: ICMP echo requests sent to a broadcast address

## MITRE ATT&CK

- T1498.001 Direct Network Flood

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
