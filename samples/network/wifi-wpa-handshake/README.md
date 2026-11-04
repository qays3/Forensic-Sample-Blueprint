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

## MITRE ATT&CK
- T1040 Network Sniffing
- T1110.002 Brute Force: Password Cracking

## Tools
Crafted deauth + EAPOL handshake (scapy, RadioTap). Wireless monitor mode capture.

## Signature
Forensic Sample Blueprint | qayssarayra.com
