# Dnscat2 Tunnel

## Scenario

DNS tunneling to a controlled domain: long random hex subdomains in TXT queries carry the C2 channel, the dnscat2 pattern.

Evidence: `dnscat2-tunnel.pcap`  
Capture date: 2024-09-15

## Timeline

~11:05  A stream of TXT queries with 40 char random subdomains under tunnel-corp.xyz.

## How to spot it

- Long random subdomains under one domain
- TXT record queries and answers
- High query rate to one resolver path
- Data encoded in the labels

## Questions

```
1. Channel?           DNS TXT tunneling
2. Domain?             tunnel-corp.xyz
3. Tell?               long random hex subdomains
4. Tool?               dnscat2
5. Technique?          DNS C2
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `dnscat2-tunnel.pcap`.
2. Open `dnscat2-tunnel.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns.qry.name contains "tunnel-corp"`  (DNS tunneling queries). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r dnscat2-tunnel.pcap -Y "dns.qry.name contains "tunnel-corp""`, and `tshark -r dnscat2-tunnel.pcap -q -z conv,ip`.
5. Confirm against the answer: Long random subdomains under one domain

## MITRE ATT&CK

- T1071.004 DNS
- T1572 Protocol Tunneling

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
