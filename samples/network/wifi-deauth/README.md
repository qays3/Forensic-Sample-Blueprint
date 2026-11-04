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

## MITRE ATT&CK
- T1498 Network Denial of Service

## Tools
Crafted 802.11 deauth frames (scapy, RadioTap). Capture is wireless monitor mode, so no wired background.

## Signature
Forensic Sample Blueprint | qayssarayra.com
