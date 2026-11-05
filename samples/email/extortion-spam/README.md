# extortion-spam

## Scenario
A sextortion/extortion spam threatens to leak data unless Bitcoin is paid, a bulk scam with no real access.

## Timeline
```
Extortion mail demanding Bitcoin within 48 hours
```

## How to spot it
- A threat plus a Bitcoin address
- A deadline to pressure payment
- Usually bulk, no real access
- Claims of compromise with no proof

## Questions
```
1. Type?              Extortion / sextortion spam
2. Demand?             0.05 BTC
3. Pressure?           48 hour deadline
4. Real access?        No (bluff)
5. Payment?            Bitcoin address
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2debb57fdc3d4e856c959d7b024eba78bfe51a5caa79c2a1e96c4bd05e60299a.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A threat plus a Bitcoin address

## MITRE ATT&CK
- T1657 Financial Theft

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
