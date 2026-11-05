# reply-chain-hijack

## Scenario
An attacker who compromised a mailbox replies inside a real existing thread, adding a malicious link, so it looks like a trusted ongoing conversation.

## Timeline
```
A reply inside a genuine Q3 contract thread, adding a malicious link
```

## How to spot it
- A reply in a real existing thread (RE: with quoted history)
- The new sender hop fails authentication
- A link added that was not in the original thread
- Thread hijacking builds trust

## Questions
```
1. Technique?         Reply chain hijack
2. Thread?             Q3 vendor contract
3. Added?              files-share-portal.com link
4. Auth on new hop?    fail
5. Why effective?      Trusted ongoing conversation
```

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
