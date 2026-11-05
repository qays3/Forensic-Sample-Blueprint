# Database Dump

## Scenario

A database is dumped over the wire from 10.10.1.66 against the MySQL server 10.10.2.70: a login then large SELECT * queries pulling whole tables.

Evidence: `database-dump.pcap`  
Capture date: 2020-12-04

## Timeline

~10:03  MySQL login then SELECT * on customer and token tables, large result sets.

## How to spot it

- Connection to a database port (3306)
- SELECT * against whole tables
- Very large result sets returned
- Sensitive tables read in bulk

## Questions

```
1. Protocol?          MySQL (3306)
2. Queries?            SELECT * whole tables
3. Tables?             customers, payment_tokens
4. Volume?             large result sets
5. ATT&CK?             T1005
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `database-dump.pcap`.
2. Open `database-dump.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `mysql || tcp.port==3306`  (the bulk SELECTs). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r database-dump.pcap -Y "mysql || tcp.port==3306"`, and `tshark -r database-dump.pcap -q -z conv,ip`.
5. Confirm against the answer: Connection to a database port (3306)

## MITRE ATT&CK

- T1005 Data from Local System
- T1213 Data from Information Repositories

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
