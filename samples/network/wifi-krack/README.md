# Wifi Krack

## Scenario

The capture shows repeated retransmissions of EAPOL message 3 of the WPA2 four-way handshake with a reused replay counter, the signature of a KRACK key reinstallation attack.

Evidence: `wifi-krack.pcap`  
Capture date: 2020-02-18

## Timeline

~09:05  EAPOL-Key message 3/4 is retransmitted several times, forcing nonce/key reuse on the client.

## How to spot it

- Repeated EAPOL message 3 of the 4-way handshake
- Reused replay counters
- Forces IV/nonce reuse on the client
- Enables decryption/replay of WPA2 traffic

## Questions

```
1. Protocol?          WPA2 EAPOL (802.11)
2. Tell?               retransmitted msg 3/4
3. Reused?             replay counter / nonce
4. Effect?             key reinstallation
5. Technique?          KRACK
```

## MITRE ATT&CK

- T1557 Adversary-in-the-Middle

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
