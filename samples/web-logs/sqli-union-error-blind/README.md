# sqli-union-error-blind

## Scenario
An attacker tests the id parameter on /products for SQL injection: error based (a quote), boolean (OR 1=1 / AND 1=2), union based (UNION SELECT), and time based (SLEEP).

## Timeline (UTC+3)
```
13:00:00  Injection attempts on /products?id= from 45.137.22.90
13:0x:xx  Quote error, boolean, UNION SELECT, SLEEP payloads
```

## How to spot it
- Single quotes and SQL keywords in a parameter value
- UNION SELECT pulling username, password columns
- A 500 error on the quote, telling the attacker it is injectable
- SLEEP for blind time based testing

## Questions
```
1. Parameter?         id on /products
2. Injection types?   error, boolean, union, time based
3. Columns targeted?  username, password from users
4. Blind technique?   SLEEP(5)
5. Error sign?        HTTP 500 on the quote
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
