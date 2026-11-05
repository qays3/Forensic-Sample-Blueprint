# Subdomain Brute Force

## Scenario

10.10.1.66 brute forced subdomains of corp-example.com against the resolver, most answers coming back NXDOMAIN with a handful resolving.

Evidence: `subdomain-brute-force.pcap`  
Capture date: 2022-12-21

## Timeline

~12:00  A burst of A queries for guessed subdomains; most return NXDOMAIN, a few resolve.

## How to spot it

- Many A queries for one domain from one host
- A high rate of NXDOMAIN answers
- Common subdomain wordlist (vpn, dev, git, jenkins)
- A few valid hits among the misses

## Questions

```
1. Scanner?           10.10.1.66
2. Domain?             corp-example.com
3. Tell?               Many NXDOMAIN answers
4. Found?              mail, vpn, api, owa, autodiscover
5. Technique?          Subdomain brute force
```

## MITRE ATT&CK

- T1595.002 Vulnerability Scanning
- T1590.002 DNS

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
