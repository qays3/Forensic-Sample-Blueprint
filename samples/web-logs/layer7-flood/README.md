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

## MITRE ATT&CK
- T1499.002 Endpoint Denial of Service: Service Exhaustion Flood

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
