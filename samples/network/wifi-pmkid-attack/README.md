# wifi-pmkid-attack

## Scenario
An attacker captures the PMKID from the first EAPOL message of a WPA2 association. Unlike the four way handshake, a PMKID attack needs no client, just one frame from the AP, and can be cracked offline.

## Timeline (UTC+3)
```
Monitor mode capture
Association request to the AP
The AP sends EAPOL M1 containing the PMKID, captured for offline cracking
```

## How to spot it
- A single EAPOL message carrying a PMKID
- No full four way handshake or client needed
- Just one frame from the AP is enough
- PMKID is cracked offline like a handshake

## Questions
```
1. Access point?     00:11:22:33:44:55
2. What is captured?  PMKID (in EAPOL M1)
3. Client needed?    No, unlike the 4-way handshake
4. Frames needed?    One from the AP
5. Next step?        Offline crack
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 134fd742c45f7db8d65bc058297ed82a8043458a172232403b2e384af60ded8c.zip -phidden`. You get `wifi-pmkid-attack.pcap`.
2. Open `wifi-pmkid-attack.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `wlan || eapol`  (802.11 management frames and EAPOL handshakes). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r wifi-pmkid-attack.pcap -Y "wlan || eapol"`, and list talkers with `tshark -r wifi-pmkid-attack.pcap -q -z conv,ip`.
5. Confirm against the answer: A single EAPOL message carrying a PMKID

## MITRE ATT&CK
- T1040 Network Sniffing
- T1110.002 Brute Force: Password Cracking

## Tools
Crafted PMKID EAPOL frame (scapy, RadioTap). Wireless monitor mode capture.

## Signature
Forensic Sample Blueprint | qayssarayra.com
