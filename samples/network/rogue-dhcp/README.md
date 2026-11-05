# rogue-dhcp

## Scenario
A rogue DHCP server (10.10.1.66) answers a client's DHCP discover alongside the real server. Its offer sets itself as the gateway and DNS, so if the client takes it, all traffic routes through the attacker.

## Timeline (UTC+3)
```
09:25:00  Client 10.10.1.80 broadcasts DHCP discover
09:25:00  Two DHCP offers come back: the real server and the rogue 10.10.1.66
          The rogue offer sets router and DNS to 10.10.1.66
```

## How to spot it
- Two DHCP servers answer the same discover
- The rogue offer sets the gateway and DNS to the attacker
- Whichever the client takes decides its routing
- An unexpected DHCP server on the segment

## Questions
```
1. Rogue server?     10.10.1.66
2. Real server?      the gateway
3. What the rogue sets?  router and DNS to itself
4. Client?           10.10.1.80
5. Risk?             All client traffic via attacker
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x c231e920d97310fdc4d4106192cad61ea1c5092fe7860fd9a90330c3a164e67e.zip -phidden`. You get `rogue-dhcp.pcap`.
2. Open `rogue-dhcp.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r rogue-dhcp.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r rogue-dhcp.pcap -q -z conv,ip`.
5. Confirm against the answer: Two DHCP servers answer the same discover

## MITRE ATT&CK
- T1557 Adversary in the Middle

## Tools
Crafted rogue DHCP offers (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
