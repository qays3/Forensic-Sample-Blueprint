# Password Policy Discovery

## Scenario

Password policy discovery from 10.10.1.66 against the DC 10.10.2.10: the SAMR interface queries the domain password policy (net accounts).

Evidence: `password-policy-discovery.pcap`  
Capture date: 2023-03-01

## Timeline

~12:00  SAMR QueryInformationDomain for the password policy.

## How to spot it

- SAMR QueryInformationDomain call
- Password policy fields returned
- Min length and lockout threshold
- Reconnaissance before password attacks

## Questions

```
1. Interface?          SAMR
2. Call?               QueryInformationDomain
3. Data?               password policy
4. Goal?               tune a password attack
5. ATT&CK?             T1201
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `password-policy-discovery.pcap`.
2. Open `password-policy-discovery.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==445`  (the policy query). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r password-policy-discovery.pcap -Y "dcerpc || tcp.port==445"`, and `tshark -r password-policy-discovery.pcap -q -z conv,ip`.
5. Confirm against the answer: SAMR QueryInformationDomain call

## MITRE ATT&CK

- T1201 Password Policy Discovery

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
