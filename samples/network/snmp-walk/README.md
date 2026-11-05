# Snmp Walk

## Scenario

10.10.1.66 walked SNMP on 10.10.2.30 using the default community string public (UDP 161), pulling system and process information.

Evidence: `snmp-walk.pcap`  
Capture date: 2025-06-14

## Timeline

~14:00  GetNext requests over UDP 161 with community public walking the MIB tree.

## How to spot it

- SNMP traffic to UDP 161 with community public
- A sequence of GetNext requests walking OIDs
- Default community string in cleartext
- System, process and user info returned

## Questions

```
1. Scanner?           10.10.1.66
2. Target?             10.10.2.30
3. Community?           public (default)
4. Port?               UDP 161
5. Technique?          SNMP walk
```

## MITRE ATT&CK

- T1590 Gather Victim Network Information
- T1046 Network Service Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
