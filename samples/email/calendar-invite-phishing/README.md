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

## MITRE ATT&CK
- T1566 Phishing

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
