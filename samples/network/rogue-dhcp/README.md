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

## MITRE ATT&CK
- T1557 Adversary in the Middle

## Tools
Crafted rogue DHCP offers (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
