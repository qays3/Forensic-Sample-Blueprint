# wpad-spoofing

## Scenario
A workstation (10.10.1.58) looks for a WPAD proxy auto config file. The name wpad has no DNS record, so it falls back to LLMNR, and an attacker (10.10.1.66) answers. The victim downloads a malicious PAC file that routes all its web traffic through the attacker, who can now read and modify it.

## Timeline (UTC+3)
```
12:05:00  Victim asks for wpad via LLMNR
12:05:00  Attacker answers as wpad (10.10.1.66)
12:05:01  Victim downloads wpad.dat, a PAC pointing at proxy 10.10.1.66:8080
12:05:xx  Victim web traffic now flows through the attacker proxy
```

## How to spot it
- A wpad lookup falls back to LLMNR and one host answers
- The PAC file (wpad.dat) sets the proxy to an internal attacker IP
- After that the victim sends web requests to that proxy
- Proxy auth and full URLs are now visible to the attacker

## Questions
```
1. Victim IP?            10.10.1.58
2. Attacker IP?          10.10.1.66
3. File downloaded?      wpad.dat (PAC file)
4. Proxy it sets?        10.10.1.66:8080
5. Impact?               Attacker sees and can change all web traffic
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 34f3658062d7b069a9084bd7e7766652efd2fd7c96a1eebb88f1f5d50723b610.zip -phidden`. You get `wpad-spoofing.pcap`.
2. Open `wpad-spoofing.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags==0x029 || tcp.flags==0x000 || tcp.flags==0x001`  (odd TCP flag combinations). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r wpad-spoofing.pcap -Y "tcp.flags==0x029 || tcp.flags==0x000 || tcp.flags==0x001"`, and list talkers with `tshark -r wpad-spoofing.pcap -q -z conv,ip`.
5. Confirm against the answer: A wpad lookup falls back to LLMNR and one host answers

## MITRE ATT&CK
- T1557.001 Adversary in the Middle: LLMNR/NBT-NS Poisoning
- T1090 Proxy

## Tools
Crafted LLMNR and WPAD PAC delivery (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
