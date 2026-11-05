# Steganography Png

## Scenario

A PNG looks normal but carries data appended after the IEND chunk, a simple image steganography trick. The appended block here is a benign marker standing in for the real hidden payload.

Host/case date: 2020-02-03  
Evidence: `logo.png` (PNG with appended data)

> Defanged for safe analysis. Attacker URLs are written `hxxp://...[.]tld`, domains are fake, and no working payload is included. The file reproduces the structure and indicators an analyst triages, not a functional sample.

## Timeline

The image renders fine; bytes after IEND hold a hidden block an analyst finds by reading past the PNG end marker.

## How to spot it

- Data present after the PNG IEND chunk
- File size larger than the image warrants
- Image still opens normally
- Carrier for a hidden payload or config

## Questions

```
1. Carrier?           PNG image
2. Hidden where?        After the IEND chunk
3. Tell?               Bytes past the end marker
4. IOC?                img-corp-cdn[.]xyz
5. Technique?          Steganography
```

## MITRE ATT&CK

- T1027.003 Steganography

## Tools

oletools (olevba, oleid), oledump, pdfid / pdf-parser, LECmd, 7-Zip, CyberChef, a hex editor. Read the evidence file in a text viewer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
