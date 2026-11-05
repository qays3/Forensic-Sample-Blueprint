# reply-to-mismatch

## Scenario
The From looks internal and authenticates, but the Reply-To points to an external address, so any reply (and the payment thread) goes to the attacker.

## Timeline
```
Internal looking approval mail with an external Reply-To
```

## How to spot it
- From is internal and passes auth
- Reply-To is a different external domain
- Replies leave the organization to the attacker
- Subtle header mismatch

## Questions
```
1. From?              procurement@corp.local (internal)
2. Reply-To?           collector@external-billing.net
3. Mismatch?           From vs Reply-To
4. Effect?             Replies go to attacker
5. Theme?              Payment approval
```

## MITRE ATT&CK
- T1566 Phishing
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
