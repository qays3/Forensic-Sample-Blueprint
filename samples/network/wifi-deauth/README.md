# wifi-deauth

## Scenario
An attacker floods 802.11 deauthentication frames to knock a client (66:77:88:99:aa:bb) off its access point (00:11:22:33:44:55). Used for denial of service or to force a reconnect so a handshake can be captured.

## Timeline (UTC+3)
```
Monitor mode capture
Burst of deauth frames spoofing the AP and client, reason code 7
Client is repeatedly disconnected
```

## How to spot it
- A flood of 802.11 deauthentication frames
- Frames spoof both the AP and the client addresses
- Reason code 7 (class 3 frame from nonassociated station) is common
- Knocks clients offline or forces a reconnect

## Questions
```
1. Access point?     00:11:22:33:44:55
2. Client?           66:77:88:99:aa:bb
3. Frame type?       802.11 deauthentication
4. Reason code?      7
5. Purpose?          DoS or force reconnect for handshake capture
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d5f0e0cad9217e59ecbbe8d9e1966a75cfed8777b2a353c32881611954b57c7f.zip -phidden`. You get `wifi-deauth.pcap`.
2. Open `wifi-deauth.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `wlan || eapol`  (802.11 management frames and EAPOL handshakes). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r wifi-deauth.pcap -Y "wlan || eapol"`, and list talkers with `tshark -r wifi-deauth.pcap -q -z conv,ip`.
5. Confirm against the answer: A flood of 802.11 deauthentication frames

## MITRE ATT&CK
- T1498 Network Denial of Service

## Tools
Crafted 802.11 deauth frames (scapy, RadioTap). Capture is wireless monitor mode, so no wired background.

## Signature
Forensic Sample Blueprint | qayssarayra.com
