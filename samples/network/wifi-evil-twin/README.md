# wifi-evil-twin

## Scenario
An attacker runs an evil twin: a rogue access point (de:ad:be:ef:00:01) broadcasting the same SSID (CorpWiFi) as the real AP (00:11:22:33:44:55). Clients may connect to the attacker, who then intercepts their traffic.

## Timeline (UTC+3)
```
Monitor mode capture
Two access points beacon the same SSID CorpWiFi from different BSSIDs
The rogue BSSID is the evil twin
```

## How to spot it
- Two BSSIDs (MACs) beaconing the same SSID
- One BSSID is unknown, the evil twin
- Clients may roam to the stronger rogue AP
- Enables interception once clients connect

## Questions
```
1. Real AP BSSID?    00:11:22:33:44:55
2. Rogue BSSID?      de:ad:be:ef:00:01
3. Shared SSID?      CorpWiFi
4. Tell?             Same SSID, two different BSSIDs
5. Risk?             Clients connect to attacker AP
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1ba4f7768e255e48348bf307506756247959a42b83bfd7b4bce69acade1301a3.zip -phidden`. You get `wifi-evil-twin.pcap`.
2. Open `wifi-evil-twin.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `wlan || eapol`  (802.11 management frames and EAPOL handshakes). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r wifi-evil-twin.pcap -Y "wlan || eapol"`, and list talkers with `tshark -r wifi-evil-twin.pcap -q -z conv,ip`.
5. Confirm against the answer: Two BSSIDs (MACs) beaconing the same SSID

## MITRE ATT&CK
- T1557 Adversary in the Middle

## Tools
Crafted evil twin beacons (scapy, RadioTap). Wireless monitor mode capture.

## Signature
Forensic Sample Blueprint | qayssarayra.com
