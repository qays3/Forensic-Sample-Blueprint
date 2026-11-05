# Kerberos Brute Force

## Scenario

A Kerberos password spray or brute force from 10.10.1.66 against the DC 10.10.2.10: many AS-REQ to port 88 returning preauth failures.

Evidence: `kerberos-brute-force.pcap`  
Capture date: 2023-12-17

## Timeline

~10:00  A burst of Kerberos AS-REQ to 88, mostly PREAUTH_FAILED errors.

## How to spot it

- Many Kerberos AS-REQ to port 88
- Mostly KDC_ERR_PREAUTH_FAILED replies
- Many usernames from one source
- Brute force or password spray

## Questions

```
1. Protocol?          Kerberos (88)
2. Message?            AS-REQ storm
3. Errors?             PREAUTH_FAILED
4. Source?             one host, many users
5. ATT&CK?             T1110
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `kerberos-brute-force.pcap`.
2. Open `kerberos-brute-force.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `kerberos || udp.port==88`  (the AS-REQ storm). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r kerberos-brute-force.pcap -Y "kerberos || udp.port==88"`, and `tshark -r kerberos-brute-force.pcap -q -z conv,ip`.
5. Confirm against the answer: Many Kerberos AS-REQ to port 88

## MITRE ATT&CK

- T1110 Brute Force

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
