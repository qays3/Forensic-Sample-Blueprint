# ack-scan

## Scenario
10.10.1.66 ran an ACK scan against 10.10.2.15 to map which ports a firewall filters.  

## How to spot it
- Bare ACK packets with no prior handshake  
- Unfiltered ports reply RST, filtered ports (139, 445) give no reply  
- Used to learn firewall rules, not open or closed state  

## Questions
```
1. Scanner IP?          10.10.1.66
2. Scan goal?           Map firewall filtering
3. Unfiltered sign?     RST reply
4. Filtered ports?      139, 445 (no reply)
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x adf79ce91ba8989a692b7093976d847e8a3b6f897fd28969af9c23a5910cad6e.zip -phidden`. You get `ack-scan.pcap`.
2. Open `ack-scan.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.ack==1 && tcp.flags.syn==0`  (bare ACK packets with no session). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ack-scan.pcap -Y "tcp.flags.ack==1 && tcp.flags.syn==0"`, and list talkers with `tshark -r ack-scan.pcap -q -z conv,ip`.
5. Confirm against the answer: Bare ACK packets with no prior handshake

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
