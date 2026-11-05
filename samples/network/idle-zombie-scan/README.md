# idle-zombie-scan

## Scenario
10.10.1.66 ran an idle (zombie) scan against 10.10.2.15, using the quiet host 10.10.1.33  
as a zombie to hide its own address.  

## How to spot it
- The attacker never sends SYN to the target directly  
- It watches the zombie IP ID field, which jumps when the target replies to spoofed SYNs  
- Spoofed SYNs to the target appear to come from the zombie  

## Questions
```
1. Real attacker?     10.10.1.66
2. Zombie host?       10.10.1.33
3. Target?            10.10.2.15
4. What is watched?   The zombie IP ID increments
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 5f3b1e9bf9423f82f9fd8e0a49f0d25cf65e6c91aed31854c37b5964c6ce62dc.zip -phidden`. You get `idle-zombie-scan.pcap`.
2. Open `idle-zombie-scan.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r idle-zombie-scan.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r idle-zombie-scan.pcap -q -z conv,ip`.
5. Confirm against the answer: The attacker never sends SYN to the target directly

## MITRE ATT&CK
T1595.001 Active Scanning  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
