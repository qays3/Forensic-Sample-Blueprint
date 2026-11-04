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

## MITRE ATT&CK
- T1557 Adversary in the Middle
- T1600 Weaken Encryption

## Tools
Crafted TLS downgrade handshake (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
