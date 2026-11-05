# Database Exfil

## Scenario

A full table is pulled over the network: a SELECT * returns a huge result set from the database server, bulk data theft over the DB protocol.

Evidence: `database-exfil.pcap`  
Capture date: 2019-06-24

## Timeline

~14:09  A SELECT * query to MySQL 3306 returning a very large result set.

## How to spot it

- A SELECT * or dump query
- A very large result set returned
- Big bytes from the DB server to one client
- Off hours or unusual client

## Questions

```
1. Protocol?          MySQL (3306)
2. Query?              SELECT * FROM customers
3. Direction?           large result to client
4. Tell?               bulk read
5. Technique?          Data export over network
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `database-exfil.pcap`.
2. Open `database-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==3306`  (The database read). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r database-exfil.pcap -Y "tcp.port==3306"`, and `tshark -r database-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: A SELECT * or dump query

## MITRE ATT&CK

- T1030 Data Transfer Size Limits
- T1213 Data from Information Repositories

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
