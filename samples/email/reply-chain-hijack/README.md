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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 136d479846a290793db551660a807ad9545926c0cbe232bd6299946afc30baa3.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A reply in a real existing thread (RE: with quoted history)

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
