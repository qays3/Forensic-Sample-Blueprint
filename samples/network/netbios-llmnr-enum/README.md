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

## MITRE ATT&CK

- T1590 Gather Victim Network Information
- T1046 Network Service Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
