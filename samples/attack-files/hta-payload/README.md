# Hta Payload

## Scenario

An HTA arrived posing as an invoice viewer. It builds a hidden PowerShell download cradle; the execution line is disabled in this sample.

Host/case date: 2023-03-27  
Evidence: `invoice.hta` (defanged HTA source)

> Defanged for safe analysis. Attacker URLs are written `hxxp://...[.]tld`, domains are fake, and no working payload is included. The file reproduces the structure and indicators an analyst triages, not a functional sample.

## Timeline

Opening via mshta would run the VBScript, launching hidden PowerShell to pull s.ps1 from a fake domain.

## How to spot it

- An .hta with VBScript and WScript.Shell
- A hidden PowerShell download cradle string
- Run via mshta.exe
- Delivered as a document lookalike

## Questions

```
1. Host binary?       mshta.exe
2. Action?             Hidden PowerShell IEX cradle
3. IOC?                update-portal-corp[.]xyz
4. Disguise?           Invoice viewer
5. Technique?          HTA attachment
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 57b4608b1887cb8bec56187d0229e52bb341342da75f80ba67c70e013cc6568b.zip -phidden` (or your archive tool). You get `invoice.hta`.
2. Open `invoice.hta` in a text editor (do not run it).
3. Pull IOCs and decode: `grep -Eoi 'hxxp[^\"> ]+|https?://[^\"> ]+' invoice.hta`, then paste any base64 or char-code blobs into CyberChef to deobfuscate.
4. Confirm against the answer: An .hta with VBScript and WScript.Shell

## MITRE ATT&CK

- T1218.005 Mshta
- T1059.001 PowerShell
- T1566.001 Spearphishing Attachment

## Tools

oletools (olevba, oleid), oledump, pdfid / pdf-parser, LECmd, 7-Zip, CyberChef, a hex editor. Read the evidence file in a text viewer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
