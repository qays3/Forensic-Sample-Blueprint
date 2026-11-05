# arp-sweep

## Scenario
10.10.1.66 ran an ARP sweep across the local subnet 10.10.1.0/24 to map live hosts.  

## How to spot it
- A burst of ARP who-has requests for every address on the subnet from one MAC  
- Only works on the local segment, so a sign of a host already inside  
- Live hosts reply with ARP is-at  

## Questions
```
1. Scanner IP?      10.10.1.66
2. Method?          ARP who-has sweep
3. Why local only?  ARP does not cross routers
4. Live hosts?      10
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3deb6128c1e70889b916727b49fe693a487c4dea5108d891d74927cfd79b1aac.zip -phidden`. You get `arp-sweep.pcap`.
2. Open `arp-sweep.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `arp`  (ARP requests and replies). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r arp-sweep.pcap -Y "arp"`, and list talkers with `tshark -r arp-sweep.pcap -q -z conv,ip`.
5. Confirm against the answer: A burst of ARP who-has requests for every address on the subnet from one MAC

## MITRE ATT&CK
T1018 Remote System Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
