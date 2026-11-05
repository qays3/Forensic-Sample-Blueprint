# ping-sweep

## Scenario
10.10.1.66 ran an ICMP ping sweep across 10.10.2.1-59 to find live hosts.  

## How to spot it
- One host sends ICMP echo requests to a whole subnet in order  
- Live hosts reply with echo reply, dead ones stay silent  

## Questions
```
1. Scanner IP?       10.10.1.66
2. Method?           ICMP echo (ping) sweep
3. Range swept?      10.10.2.1 to 10.10.2.59
4. How many live?    9
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 5829246a3bc3e431a2e6f3bf60577099cb1a22e2044354a620e076364cb49a61.zip -phidden`. You get `ping-sweep.pcap`.
2. Open `ping-sweep.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp`  (ICMP echo traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ping-sweep.pcap -Y "icmp"`, and list talkers with `tshark -r ping-sweep.pcap -q -z conv,ip`.
5. Confirm against the answer: One host sends ICMP echo requests to a whole subnet in order

## MITRE ATT&CK
T1018 Remote System Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
