# wifi-wpa-handshake

## Scenario
An attacker deauthenticates a client to force it to reconnect, then captures the WPA2 four way handshake (EAPOL). The handshake can be cracked offline to recover the WiFi passphrase.

## Timeline (UTC+3)
```
Monitor mode capture
Deauth burst forces the client to reconnect
The EAPOL four way handshake (M1 to M4) is captured
```

## How to spot it
- A deauth burst right before a reconnect
- Four EAPOL frames (the WPA2 four way handshake)
- Capturing M1 to M4 is enough to crack offline
- Classic WiFi passphrase attack setup

## Questions
```
1. Access point?     00:11:22:33:44:55
2. Client?           66:77:88:99:aa:bb
3. What is captured?  WPA2 4-way handshake (EAPOL)
4. How forced?       Deauth to trigger reconnect
5. Next step?        Offline crack of the passphrase
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x f82c4425eb9c4fb3c71efb15e1293f8e464e4212d96fe6269c623c5d95dbd741.zip -phidden`. You get `wifi-wpa-handshake.pcap`.
2. Open `wifi-wpa-handshake.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `wlan || eapol`  (802.11 management frames and EAPOL handshakes). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r wifi-wpa-handshake.pcap -Y "wlan || eapol"`, and list talkers with `tshark -r wifi-wpa-handshake.pcap -q -z conv,ip`.
5. Confirm against the answer: A deauth burst right before a reconnect

## MITRE ATT&CK
- T1040 Network Sniffing
- T1110.002 Brute Force: Password Cracking

## Tools
Crafted deauth + EAPOL handshake (scapy, RadioTap). Wireless monitor mode capture.

## Signature
Forensic Sample Blueprint | qayssarayra.com
