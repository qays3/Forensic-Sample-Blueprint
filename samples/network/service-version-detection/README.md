# service-version-detection

## Scenario
10.10.1.66 ran service and version detection against open ports on 10.10.2.15, grabbing  
banners to identify the software.  

## How to spot it
- Short connections to known service ports that pull a banner then probe  
- Banners reveal OpenSSH 8.9, Apache 2.4.52, vsFTPd 3.0.5, Postfix, MySQL 8.0.34  
- Follows a port scan in a real attack  

## Questions
```
1. Scanner IP?        10.10.1.66
2. What is grabbed?   Service banners and versions
3. SSH version?       OpenSSH_8.9p1
4. Web server?        Apache 2.4.52
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1a558ca73fb0e7e6ad844477a011d0e72638fbf77909b526a125883dd8a51ce7.zip -phidden`. You get `service-version-detection.pcap`.
2. Open `service-version-detection.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (SYN probes across ports). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r service-version-detection.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and list talkers with `tshark -r service-version-detection.pcap -q -z conv,ip`.
5. Confirm against the answer: Short connections to known service ports that pull a banner then probe

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
