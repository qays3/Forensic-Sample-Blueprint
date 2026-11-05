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

## MITRE ATT&CK

- T1218.005 Mshta
- T1059.001 PowerShell
- T1566.001 Spearphishing Attachment

## Tools

oletools (olevba, oleid), oledump, pdfid / pdf-parser, LECmd, 7-Zip, CyberChef, a hex editor. Read the evidence file in a text viewer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
