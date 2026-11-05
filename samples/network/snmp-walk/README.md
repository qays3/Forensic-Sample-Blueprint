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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 92fed85de78bdb050a682c89635cbfc0d6b6cff2ea2545ce51e41759a1cc507e.zip -phidden`. You get `snmp-walk.pcap`.
2. Open `snmp-walk.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `snmp`  (SNMP get/getnext with the community string). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r snmp-walk.pcap -Y "snmp"`, and list talkers with `tshark -r snmp-walk.pcap -q -z conv,ip`.
5. Confirm against the answer: SNMP traffic to UDP 161 with community public

## MITRE ATT&CK

- T1590 Gather Victim Network Information
- T1046 Network Service Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
