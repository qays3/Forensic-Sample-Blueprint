# Nmap Nse Scan

## Scenario

The host 10.10.1.66 ran Nmap NSE HTTP scripts against the web server 10.10.2.20, requesting sensitive paths with the Nmap Scripting Engine user agent.

Evidence: `nmap-nse-scan.pcap`  
Capture date: 2023-11-02

## Timeline

~11:01  10.10.1.66 sends scripted HTTP probes (/.git/HEAD, /.env, /wp-login.php) with an NSE user agent.

## How to spot it

- HTTP requests carrying an Nmap Scripting Engine user agent
- Probes for sensitive paths (.git, .env, admin)
- Many short connections from one source
- Mix of 200 and 404 in seconds

## Questions

```
1. Scanner?           10.10.1.66
2. Target?             10.10.2.20 (web)
3. Tell?               Nmap NSE user agent
4. Probed?             .git/HEAD, .env, wp-login
5. Technique?          Active scanning (NSE)
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 78d489d6230f232bc5adf66ea2507a072732eba7832ff804e83286c3160ca57f.zip -phidden`. You get `nmap-nse-scan.pcap`.
2. Open `nmap-nse-scan.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r nmap-nse-scan.pcap -Y "http || tls"`, and list talkers with `tshark -r nmap-nse-scan.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP requests carrying an Nmap Scripting Engine user agent

## MITRE ATT&CK

- T1595.002 Vulnerability Scanning
- T1592 Gather Victim Host Information

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
