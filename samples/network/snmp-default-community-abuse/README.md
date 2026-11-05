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

## MITRE ATT&CK

- T1078.001 Default Accounts
- T1590 Gather Victim Network Information

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
