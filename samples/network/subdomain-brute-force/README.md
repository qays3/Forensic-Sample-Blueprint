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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d9416e6921ecd664cc5752bc36b4d133a8566fc0268e738c4e50aa66039f05ad.zip -phidden`. You get `subdomain-brute-force.pcap`.
2. Open `subdomain-brute-force.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r subdomain-brute-force.pcap -Y "dns"`, and list talkers with `tshark -r subdomain-brute-force.pcap -q -z conv,ip`.
5. Confirm against the answer: Many A queries for one domain from one host

## MITRE ATT&CK

- T1595.002 Vulnerability Scanning
- T1590.002 DNS

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
