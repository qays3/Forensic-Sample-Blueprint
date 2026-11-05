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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 16b5ce616fe6b2f60fed04cf52dcf773397c17d18f50e2e8011f50628e42ad71.zip -phidden`. You get `arp-spoofing-mitm.pcap`.
2. Open `arp-spoofing-mitm.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `arp`  (ARP requests and replies). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r arp-spoofing-mitm.pcap -Y "arp"`, and list talkers with `tshark -r arp-spoofing-mitm.pcap -q -z conv,ip`.
5. Confirm against the answer: Unsolicited ARP replies (gratuitous ARP) repeated every couple of seconds

## MITRE ATT&CK
- T1557.002 Adversary in the Middle: ARP Cache Poisoning

## Tools
Crafted ARP poisoning (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
