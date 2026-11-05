# teardrop-fragmentation

## Scenario
An attacker (10.10.1.66) sends overlapping, malformed IP fragments (teardrop) to 10.10.2.20. Buggy reassembly can crash or hang the target.

## Timeline (UTC+3)
```
10:30:00  Overlapping fragments with bad offsets sent to 10.10.2.20
10:30:xx  Repeated malformed fragment pairs
```

## How to spot it
- IP fragments with overlapping or impossible offsets
- Same IP id with fragments that cannot be reassembled cleanly
- Aimed at weak fragment reassembly code
- Can crash or hang older stacks

## Questions
```
1. Attacker?         10.10.1.66
2. Target?           10.10.2.20
3. Technique?         Overlapping IP fragments (teardrop)
4. Tell?             Bad/overlapping fragment offsets, same IP id
5. Effect?           Crash or hang on reassembly
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 96bc368374da06c16a282dc34984f8217590d8004d6a87e6bf5cec7e43bd7a89.zip -phidden`. You get `teardrop-fragmentation.pcap`.
2. Open `teardrop-fragmentation.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r teardrop-fragmentation.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r teardrop-fragmentation.pcap -q -z conv,ip`.
5. Confirm against the answer: IP fragments with overlapping or impossible offsets

## MITRE ATT&CK
- T1499.001 Endpoint Denial of Service: OS Exhaustion Flood

## Tools
Crafted teardrop fragments (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
