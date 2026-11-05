# xmas-null-fin-scans

## Scenario
10.10.1.66 ran three stealth scans against 10.10.2.15: Xmas (FIN+PSH+URG), NULL (no flags)  
and FIN (FIN only).  

## How to spot it
- TCP packets with odd or empty flag combos, never a normal SYN  
- Closed ports reply RST, open or filtered ports stay silent  
- Three waves, one per scan type  

## Questions
```
1. Scanner IP?        10.10.1.66
2. Three scan types?  Xmas, NULL, FIN
3. Xmas flags?        FIN, PSH, URG set together
4. Closed reply?      RST
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 9e7ce49872d4f781e7cf664acdd03aabe8f0543b21d752b67879e7df1ce69d4b.zip -phidden`. You get `xmas-null-fin-scans.pcap`.
2. Open `xmas-null-fin-scans.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags==0x029 || tcp.flags==0x000 || tcp.flags==0x001`  (odd TCP flag combinations). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r xmas-null-fin-scans.pcap -Y "tcp.flags==0x029 || tcp.flags==0x000 || tcp.flags==0x001"`, and list talkers with `tshark -r xmas-null-fin-scans.pcap -q -z conv,ip`.
5. Confirm against the answer: TCP packets with odd or empty flag combos, never a normal SYN

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
