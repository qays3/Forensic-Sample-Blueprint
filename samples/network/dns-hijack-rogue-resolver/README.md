# Dns Hijack Rogue Resolver

## Scenario

The client is using a changed DNS resolver that returns an attacker IP for banking and login domains while legitimate domains still resolve, a rogue resolver hijack.

Evidence: `dns-hijack-rogue-resolver.pcap`  
Capture date: 2019-03-10

## Timeline

~11:01  Several domains resolve to the same attacker IP (185.220.101.9) from a non standard resolver.

## How to spot it

- Multiple unrelated domains resolving to one IP
- Very low TTL on the poisoned answers
- Resolver is not the corporate DNS server
- Sensitive domains redirected, others normal

## Questions

```
1. Rogue resolver?    10.10.1.5 (changed)
2. Poisoned to?         185.220.101.9
3. Domains?            bank, login, office
4. Tell?               many domains, one IP, low TTL
5. Technique?          DNS hijack
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 0d013bdce8c34dc71dfc0f5c0ae879687cc13e82ec0e024e90bcbfe7e41e58ae.zip -phidden`. You get `dns-hijack-rogue-resolver.pcap`.
2. Open `dns-hijack-rogue-resolver.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r dns-hijack-rogue-resolver.pcap -Y "dns"`, and list talkers with `tshark -r dns-hijack-rogue-resolver.pcap -q -z conv,ip`.
5. Confirm against the answer: Multiple unrelated domains resolving to one IP

## MITRE ATT&CK

- T1584.002 DNS Server
- T1557 Adversary-in-the-Middle

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
