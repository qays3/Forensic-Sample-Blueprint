# arp-spoofing-mitm

## Scenario
An attacker (10.10.1.66) poisons the ARP caches of a victim (10.10.1.50) and the gateway, claiming to be each to the other. Both now send their traffic through the attacker, a man in the middle.

## Timeline (UTC+3)
```
09:20:00  Attacker starts sending gratuitous ARP replies
09:2x:xx  Tells 10.10.1.50 that the gateway is at the attacker MAC, and the gateway that 10.10.1.50 is too
```

## How to spot it
- Unsolicited ARP replies (gratuitous ARP) repeated every couple of seconds
- One MAC claims to own two IPs (the victim and the gateway)
- Victim and gateway IP now map to the attacker MAC
- Classic man in the middle setup on the LAN

## Questions
```
1. Attacker IP/MAC?   10.10.1.66
2. Victim?            10.10.1.50
3. Other poisoned?    The gateway
4. ARP type?          Unsolicited replies (op=2)
5. Goal?              Man in the middle
```

## MITRE ATT&CK
- T1557.002 Adversary in the Middle: ARP Cache Poisoning

## Tools
Crafted ARP poisoning (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
