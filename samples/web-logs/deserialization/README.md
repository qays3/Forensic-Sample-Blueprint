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

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
