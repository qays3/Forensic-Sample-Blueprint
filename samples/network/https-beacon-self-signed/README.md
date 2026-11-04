# https-beacon-self-signed

## Scenario
A host (10.10.1.77) beacons over HTTPS to 193.42.33.8 (telemetry-edge-cloud.com) every ~5 minutes. The TLS handshake uses a self signed certificate and an unusual client hello, so the JA3 fingerprint stands out even though the payload is encrypted.

## Timeline (UTC+3)
```
09:15:00  First TLS beacon to telemetry-edge-cloud.com (193.42.33.8)
all day   Short TLS sessions about every 5 minutes, ~94 of them
```

## How to spot it
- Regular short TLS sessions to one host, same size each time
- Self signed cert and an odd client hello (JA3/JA4 fingerprint)
- Young domain only this host talks to
- Payload is encrypted, so rely on timing, size and the TLS fingerprint

## Questions
```
1. Beaconing host?   10.10.1.77
2. C2?               193.42.33.8 (telemetry-edge-cloud.com)
3. Interval?         ~5 minutes
4. Cert type?        Self signed
5. What identifies it encrypted?  JA3/JA4 TLS fingerprint, timing, size
```

## MITRE ATT&CK
- T1573.002 Encrypted Channel: Asymmetric Cryptography
- T1071.001 Web Protocols

## Tools
Crafted HTTPS beacon with TLS records (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
