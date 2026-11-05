# Html Smuggling

## Scenario

An HTML attachment smuggles a container file by reconstructing it in the browser from an embedded base64 blob, so the download never crosses the proxy and carries no Mark of the Web.

Host/case date: 2022-11-06  
Evidence: `statement.html` (defanged HTML smuggling page)

> Defanged for safe analysis. Attacker URLs are written `hxxp://...[.]tld`, domains are fake, and no working payload is included. The file reproduces the structure and indicators an analyst triages, not a functional sample.

## Timeline

Opening the HTML runs JS that rebuilds statement.iso client-side and offers it for download, bypassing network inspection and MOTW.

## How to spot it

- A large embedded base64 blob in an HTML file
- Client side Blob reconstruction and forced download
- Output is an .iso/.zip in Downloads
- No MOTW because the bytes never came over HTTP

## Questions

```
1. Technique?         HTML smuggling
2. Blob?               Base64 ISO/ZIP in the page
3. Output?             statement.iso in Downloads
4. Evasion?            No proxy inspection, no MOTW
5. ATT&CK?             T1027.006
```

## MITRE ATT&CK

- T1027.006 HTML Smuggling
- T1566.001 Spearphishing Attachment

## Tools

oletools (olevba, oleid), oledump, pdfid / pdf-parser, LECmd, 7-Zip, CyberChef, a hex editor. Read the evidence file in a text viewer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
