# calendar-invite-phishing

## Scenario
A phishing email carries a calendar invite (.ics) with a malicious join link, abusing the trust people place in meeting invitations.

## Timeline
```
Calendar invite (.ics) with a malicious join link
```

## How to spot it
- An .ics calendar invite attachment
- A join link to an attacker domain
- Invites auto add to calendars, raising trust
- Meeting lure

## Questions
```
1. Attachment?        invite.ics
2. Link?               corp-calendar.com/join (fake)
3. Technique?          Calendar invite phishing
4. Auth?               fail
5. Why effective?      Trust in invites
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2bbd60bfd6ddd3c1a1a237814546e62fd8b03ccd74aeb97c1f6559e46bf3ece2.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: An .ics calendar invite attachment

## MITRE ATT&CK
- T1566 Phishing

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
