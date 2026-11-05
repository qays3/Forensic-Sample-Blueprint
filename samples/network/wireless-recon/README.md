# Wireless Recon

## Scenario

A monitor mode capture collected 802.11 management frames: beacons advertising corporate SSIDs and probe requests from clients leaking the networks they have joined before.

Evidence: `wireless-recon.pcap`  
Capture date: 2021-06-10

## Timeline

~09:10  Beacon frames from CorpWiFi/CorpGuest and probe requests revealing client preferred network lists.

## How to spot it

- 802.11 beacon frames advertising SSIDs and BSSIDs
- Probe requests listing previously joined SSIDs
- Client MAC addresses exposed
- Management frames captured in monitor mode

## Questions

```
1. Capture type?      802.11 management frames
2. APs seen?           CorpWiFi, CorpGuest, eduroam
3. Leaked by clients?   Preferred network list (probes)
4. Identifiers?         BSSID and client MACs
5. Technique?          Wireless recon
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 108e6786e10e139b8c62ab396f9cc03410d0a05dd62b85be017ec8e46ddf0db0.zip -phidden`. You get `wireless-recon.pcap`.
2. Open `wireless-recon.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `wlan || eapol`  (802.11 management frames and EAPOL handshakes). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r wireless-recon.pcap -Y "wlan || eapol"`, and list talkers with `tshark -r wireless-recon.pcap -q -z conv,ip`.
5. Confirm against the answer: 802.11 beacon frames advertising SSIDs and BSSIDs

## MITRE ATT&CK

- T1595 Active Scanning
- T1590 Gather Victim Network Information

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
