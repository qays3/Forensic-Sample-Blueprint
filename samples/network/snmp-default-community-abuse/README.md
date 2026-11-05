# Snmp Default Community Abuse

## Scenario

A device answers SNMP with the default community strings public and private, letting an attacker read its configuration and, with private, potentially write it.

Evidence: `snmp-default-community-abuse.pcap`  
Capture date: 2023-10-01

## Timeline

~12:08  SNMP requests with community public and private to a router, returning configuration data.

## How to spot it

- SNMP with community public and private
- A network device (router/switch) responding
- Configuration leaked, including secrets
- Write community exposes config changes

## Questions

```
1. Protocol?          SNMP (UDP 161)
2. Communities?         public, private (default)
3. Target?             router 10.10.2.1
4. Leaked?              running-config, secrets
5. Technique?          Default community abuse
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 804b0b4ca2e5ae4b4250a66f9f3884e95e9f1f86a0c4a0bc06c3b85c9c9294ff.zip -phidden`. You get `snmp-default-community-abuse.pcap`.
2. Open `snmp-default-community-abuse.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `snmp`  (SNMP get/getnext with the community string). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r snmp-default-community-abuse.pcap -Y "snmp"`, and list talkers with `tshark -r snmp-default-community-abuse.pcap -q -z conv,ip`.
5. Confirm against the answer: SNMP with community public and private

## MITRE ATT&CK

- T1078.001 Default Accounts
- T1590 Gather Victim Network Information

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
