# Netbios Llmnr Enum

## Scenario

10.10.1.66 issued NetBIOS name service (UDP 137) and LLMNR (UDP 5355) name queries to enumerate hosts and look for WPAD, the same traffic that enables responder poisoning.

Evidence: `netbios-llmnr-enum.pcap`  
Capture date: 2025-02-20

## Timeline

~12:15  Broadcast NBNS and multicast LLMNR queries for host and WPAD names.

## How to spot it

- NBNS queries to UDP 137 broadcast
- LLMNR queries to 224.0.0.252 UDP 5355
- A WPAD lookup among the names
- Name resolution fishing on the local segment

## Questions

```
1. Scanner?           10.10.1.66
2. Protocols?           NBNS (137), LLMNR (5355)
3. Notable name?        WPAD
4. Risk?               Enables responder poisoning
5. Technique?          NetBIOS/LLMNR enum
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1673385d2ad6c0ef28c5e8f72f737c0f435e70749cb7d7b1b5f533b1050d5c94.zip -phidden`. You get `netbios-llmnr-enum.pcap`.
2. Open `netbios-llmnr-enum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `nbns || llmnr`  (NBNS and LLMNR name queries). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r netbios-llmnr-enum.pcap -Y "nbns || llmnr"`, and list talkers with `tshark -r netbios-llmnr-enum.pcap -q -z conv,ip`.
5. Confirm against the answer: NBNS queries to UDP 137 broadcast

## MITRE ATT&CK

- T1590 Gather Victim Network Information
- T1046 Network Service Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
