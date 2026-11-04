# log4shell-strings

## Scenario
An attacker sends Log4Shell (JNDI) strings in the URL and User-Agent, trying to make a vulnerable Log4j backend connect to an attacker LDAP server.

## Timeline (UTC+3)
```
15:15:00  ${jndi:ldap://...} in User-Agent and query from 45.137.22.90
15:15:xx  Followed by an outbound LDAP callback if vulnerable
```

## How to spot it
- ${jndi:ldap://} or ${jndi:dns://} strings in headers or params
- The User-Agent itself carries the payload
- An outbound LDAP/DNS callback to the attacker if hit
- Log4Shell targets logging of attacker input

## Questions
```
1. Attack?            Log4Shell (Log4j JNDI)
2. Payload location?  User-Agent and /search?q=
3. Callback proto?    LDAP (jndi:ldap)
4. Attacker host?     45.137.22.90
5. Why headers?       They get logged by Log4j
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
