# tls-downgrade

## Scenario
An attacker forces a TLS connection down to an old version (TLS 1.0) and a weak cipher (RC4-MD5) by tampering with the handshake, making the session crackable.

## Timeline (UTC+3)
```
10:05:00  ClientHello offers only old TLS 1.0 and weak ciphers
10:05:xx  ServerHello agrees to RC4-MD5
```

## How to spot it
- A TLS handshake negotiating an old version (TLS 1.0 or SSLv3)
- Weak ciphers like RC4-MD5 or DES chosen
- Modern clients should never offer these
- A sign of downgrade tampering on the path

## Questions
```
1. Victim?           10.10.1.50
2. Version forced?   TLS 1.0
3. Weak cipher?      RC4-MD5
4. Why bad?          Crackable, breaks confidentiality
5. Cause?            Handshake tampering / downgrade
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x ee8bb563729248485a3a4ff17b48438c2e529804b0044cd2167b9e91e62373c6.zip -phidden`. You get `tls-downgrade.pcap`.
2. Open `tls-downgrade.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r tls-downgrade.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r tls-downgrade.pcap -q -z conv,ip`.
5. Confirm against the answer: A TLS handshake negotiating an old version (TLS 1.0 or SSLv3)

## MITRE ATT&CK
- T1557 Adversary in the Middle
- T1600 Weaken Encryption

## Tools
Crafted TLS downgrade handshake (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
