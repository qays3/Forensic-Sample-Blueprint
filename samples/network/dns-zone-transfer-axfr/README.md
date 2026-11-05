# Dns Zone Transfer Axfr

## Scenario

10.10.1.66 requested a full DNS zone transfer (AXFR) of corp-example.com over TCP 53 and the misconfigured server returned the entire zone.

Evidence: `dns-zone-transfer-axfr.pcap`  
Capture date: 2024-04-22

## Timeline

~13:00  TCP 53 connection, an AXFR query, then the full zone in the response.

## How to spot it

- An AXFR query over TCP 53 (not UDP)
- The server returns every record in the zone
- One request exposes all internal hostnames
- AXFR should be restricted to secondaries

## Questions

```
1. Scanner?           10.10.1.66
2. Query type?          AXFR (zone transfer)
3. Transport?           TCP 53
4. Result?             Full zone disclosed
5. Technique?          DNS zone transfer
```

## MITRE ATT&CK

- T1590.002 DNS
- T1595 Active Scanning

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
