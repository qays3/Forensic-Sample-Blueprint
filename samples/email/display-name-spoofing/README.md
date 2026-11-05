# display-name-spoofing

## Scenario
The display name shows a real manager (Khalid Mansour) but the actual address is a random Gmail, a display name spoof opening a BEC approach.

## Timeline
```
Mail that looks like it is from the manager but is a random Gmail address
```

## How to spot it
- Display name is a known executive, address is a free mail account
- Authentication passes (it really is Gmail)
- Vague urgent request to open a dialogue
- No link or attachment yet, social engineering

## Questions
```
1. Shown name?        Khalid Mansour (manager)
2. Real address?       random5521@gmail.com
3. Auth?               Passes (genuine Gmail)
4. Technique?          Display name spoofing
5. Goal?               Start a BEC
```

## MITRE ATT&CK
- T1534 Internal Spearphishing
- T1656 Impersonation

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
