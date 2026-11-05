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

## MITRE ATT&CK

- T1595.002 Vulnerability Scanning
- T1592 Gather Victim Host Information

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
