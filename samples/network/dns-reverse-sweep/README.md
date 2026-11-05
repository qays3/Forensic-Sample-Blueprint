# Dns Reverse Sweep

## Scenario

10.10.1.66 swept the 10.10.2.0/24 range with reverse (PTR) lookups to map internal hostnames.

Evidence: `dns-reverse-sweep.pcap`  
Capture date: 2019-06-05

## Timeline

~10:08  Sequential PTR queries for 10.10.2.1 through .39; some resolve to server names.

## How to spot it

- Many PTR queries walking an in-addr.arpa range
- Sequential host octets from one source
- Hostnames leak for live servers
- Reverse mapping of an internal subnet

## Questions

```
1. Scanner?           10.10.1.66
2. Query type?          PTR (reverse)
3. Range?               10.10.2.0/24
4. Goal?               Map internal hostnames
5. Technique?          Reverse lookup sweep
```

## MITRE ATT&CK

- T1590 Gather Victim Network Information
- T1595 Active Scanning

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
