# Obfuscated Jscript

## Scenario

A .js attachment was obfuscated with split strings and char codes. This is the analyst deobfuscation: it builds a URL, downloads an EXE to %TEMP%, and runs it.

Host/case date: 2024-04-08  
Evidence: `deobfuscated.js` (deobfuscated JScript)

> Defanged for safe analysis. Attacker URLs are written `hxxp://...[.]tld`, domains are fake, and no working payload is included. The file reproduces the structure and indicators an analyst triages, not a functional sample.

## Timeline

wscript runs the JS, which reassembles the URL, downloads a.exe and executes it.

## How to spot it

- String splitting and array joins to hide a URL
- ActiveX XMLHTTP + ADODB.Stream download pattern
- WScript.Shell Run of the dropped EXE
- Run by wscript/cscript

## Questions

```
1. Host binary?       wscript.exe
2. Hidden how?          Split strings / arrays
3. Action?             Download + Run a.exe
4. IOC?                pay-corp-cdn[.]xyz
5. Technique?          Obfuscated script
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3bd511c6ae237c0997dae476aa8b4ca31bf1ee548b8f7af12fdb9be33b59c57d.zip -phidden` (or your archive tool). You get `deobfuscated.js`.
2. Open `deobfuscated.js` in a text editor (do not run it).
3. Pull IOCs and decode: `grep -Eoi 'hxxp[^\"> ]+|https?://[^\"> ]+' deobfuscated.js`, then paste any base64 or char-code blobs into CyberChef to deobfuscate.
4. Confirm against the answer: String splitting and array joins to hide a URL

## MITRE ATT&CK

- T1027 Obfuscated Files or Information
- T1059.007 JavaScript

## Tools

oletools (olevba, oleid), oledump, pdfid / pdf-parser, LECmd, 7-Zip, CyberChef, a hex editor. Read the evidence file in a text viewer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
