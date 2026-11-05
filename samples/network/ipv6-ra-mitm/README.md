# ipv6-ra-mitm

## Scenario
An attacker (10.10.1.66) sends rogue IPv6 Router Advertisements (mitm6 style). Windows prefers IPv6, so clients take the attacker as their IPv6 router and DNS, letting the attacker intercept traffic even on an IPv4 network.

## Timeline (UTC+3)
```
09:40:00  Rogue ICMPv6 Router Advertisements start from fe80::dead:beef
09:40:xx  Sent to all-nodes (ff02::1) every few seconds
```

## How to spot it
- ICMPv6 Router Advertisements from an unexpected host
- Clients get an IPv6 gateway and DNS they did not have before
- Windows prefers IPv6, so this quietly takes over name resolution
- Common even on networks thought to be IPv4 only

## Questions
```
1. Attacker?         10.10.1.66 (link local fe80::dead:beef)
2. Technique?         Rogue IPv6 Router Advertisement (mitm6)
3. Sent to?          ff02::1 (all nodes)
4. Why effective?    Windows prefers IPv6
5. What it hijacks?  IPv6 gateway and DNS
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2280ef793b41a309346718ea988af28be683c57fb685df88502e9a6b677e074e.zip -phidden`. You get `ipv6-ra-mitm.pcap`.
2. Open `ipv6-ra-mitm.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ipv6-ra-mitm.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r ipv6-ra-mitm.pcap -q -z conv,ip`.
5. Confirm against the answer: ICMPv6 Router Advertisements from an unexpected host

## MITRE ATT&CK
- T1557 Adversary in the Middle

## Tools
Crafted rogue IPv6 RA (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
