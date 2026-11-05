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

## MITRE ATT&CK

- T1595 Active Scanning
- T1590 Gather Victim Network Information

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
