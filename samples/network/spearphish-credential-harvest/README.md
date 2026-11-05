# Spearphish Credential Harvest

## Scenario

A victim follows a spearphishing link to a fake single sign on page and submits credentials, which are POSTed to the attacker then redirected to the real site.

Evidence: `spearphish-credential-harvest.pcap`  
Capture date: 2025-02-04

## Timeline

~10:08  DNS for a lookalike SSO domain, the fake login page, then a POST of credentials.

## How to spot it

- DNS for a lookalike SSO or login domain
- A fake login form served
- Credentials POSTed to a /collect style endpoint
- A redirect to the real site to hide the theft

## Questions

```
1. Technique?         Credential harvesting
2. Lure domain?         login-portal-sso.example
3. Tell?               POST to /collect then redirect
4. Stolen?             submitted credentials
5. ATT&CK?             T1566.002
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `spearphish-credential-harvest.pcap`.
2. Open `spearphish-credential-harvest.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.method=="POST"`  (the credential POST). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r spearphish-credential-harvest.pcap -Y "http.request.method=="POST""`, and `tshark -r spearphish-credential-harvest.pcap -q -z conv,ip`.
5. Confirm against the answer: DNS for a lookalike SSO or login domain

## MITRE ATT&CK

- T1566.002 Spearphishing Link
- T1056 Input Capture

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
