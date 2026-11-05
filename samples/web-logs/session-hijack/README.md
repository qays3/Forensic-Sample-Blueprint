# session-hijack

## Scenario
The same session cookie is suddenly used from a new external IP and a different device, a sign of session hijacking.

## Timeline (UTC+3)
```
13:20:00  Session cookie seen from new IP 45.137.22.90
13:20:xx  Same session, different country and device
```

## How to spot it
- A session that jumps to a new IP and user agent
- No login event for the new location
- Impossible travel for one session
- Cookie reused from an unexpected place

## Questions
```
1. Attack?            Session hijacking
2. New IP?            45.137.22.90
3. Tell?              Same cookie, new IP/device, no login
4. Pages?             /dashboard, /profile
5. Cause?             Stolen session cookie
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 916bfb0410bb191c0472438c3c1b6fd0421b2a1b8ba77f72ed10d0e2c854a040.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A session that jumps to a new IP and user agent

## MITRE ATT&CK
- T1539 Steal Web Session Cookie

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
