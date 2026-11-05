# deserialization

## Scenario
An attacker sends a crafted serialized object to an endpoint that deserializes user input, aiming for remote code execution (Java/PHP/.NET gadget).

## Timeline (UTC+3)
```
15:05:00  POST /api/v1/session with a serialized gadget payload
15:05:xx  500 then 200 as the gadget lands
```

## How to spot it
- POST bodies carrying serialized objects (rO0, O:, base64 gadgets)
- An endpoint that deserializes user input
- 500 errors then success as the chain works
- RCE risk from insecure deserialization

## Questions
```
1. Endpoint?          /api/v1/session
2. Attack?            Insecure deserialization
3. Goal?              Remote code execution
4. Payload?           Serialized gadget (body)
5. Error sign?        500 during tuning
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x ccb47f505ed7914fbd7f04fbc885c36303ac2522bf63eeaa2988ca7e31beba65.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: POST bodies carrying serialized objects (rO0, O:, base64 gadgets)

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
