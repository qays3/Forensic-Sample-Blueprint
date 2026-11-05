# layer7-flood

## Scenario
An attacker floods an expensive search endpoint with hundreds of requests per second (layer 7 denial of service), exhausting the app.

## Timeline (UTC+3)
```
16:00:00  Flood of /search requests from 185.220.101.7
16:00:xx  Hundreds per second on a heavy endpoint
```

## How to spot it
- A very high request rate to one heavy endpoint
- Random query values to defeat caching
- One or few IPs driving huge volume
- App slows or errors under load

## Questions
```
1. Attack?            Layer 7 (application) DoS
2. Endpoint?          /search (expensive)
3. Rate?              Hundreds per second
4. Cache evasion?     Random q values
5. Source?            185.220.101.7
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4277537f70ae8d1cb1b86cb038072b6b960e5577da3c09697d78dbc63f8b1bb5.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A very high request rate to one heavy endpoint

## MITRE ATT&CK
- T1499.002 Endpoint Denial of Service: Service Exhaustion Flood

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
