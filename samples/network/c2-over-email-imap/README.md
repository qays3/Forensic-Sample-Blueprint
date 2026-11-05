# C2 Over Email Imap

## Scenario

Malware polls an attacker mailbox over IMAP, reading tasks from email bodies and sending results back, email as a C2 channel.

Evidence: `c2-over-email-imap.pcap`  
Capture date: 2021-07-02

## Timeline

~13:06  Repeated IMAP login, SELECT INBOX, FETCH of a task email on a timer.

## How to spot it

- Regular IMAP polling by a non mail client
- Login to an external attacker mailbox
- Base64 commands in email bodies
- Fixed interval FETCH

## Questions

```
1. Channel?           IMAP (143)
2. Mailbox?            attacker-mail.xyz
3. Pattern?            poll, FETCH task, reply
4. Encoding?            base64 in body
5. Technique?          C2 over email
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `c2-over-email-imap.pcap`.
2. Open `c2-over-email-imap.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==143 || imap`  (IMAP polling of the task mailbox). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r c2-over-email-imap.pcap -Y "tcp.port==143 || imap"`, and `tshark -r c2-over-email-imap.pcap -q -z conv,ip`.
5. Confirm against the answer: Regular IMAP polling by a non mail client

## MITRE ATT&CK

- T1071.003 Mail Protocols
- T1102 Web Service

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
