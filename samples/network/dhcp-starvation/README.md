# dhcp-starvation

## Scenario
An attacker (10.10.1.66) floods the DHCP server with discover packets using thousands of spoofed MAC addresses, draining the address pool so legitimate clients cannot get a lease.

## Timeline (UTC+3)
```
09:30:00  Flood of DHCP discover begins, each with a different fake MAC
09:30:xx  300+ discovers in seconds, exhausting the pool
```

## How to spot it
- A flood of DHCP discover packets in a short time
- Each request uses a different, random source MAC
- The DHCP pool drains, denying real clients
- Often paired with a rogue DHCP server next

## Questions
```
1. Attacker?         10.10.1.66
2. Flood type?       DHCP discover
3. What varies?      The source MAC on each packet
4. Effect?           Address pool exhausted (DoS)
5. Often paired with?  Rogue DHCP
```

## MITRE ATT&CK
- T1498 Network Denial of Service
- T1557 Adversary in the Middle

## Tools
Crafted DHCP starvation flood (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
