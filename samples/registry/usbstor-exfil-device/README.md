# Usbstor Exfil Device

## Scenario

USBSTOR and Portable Devices keys record a USB mass storage device plugged into the host, the registry trace behind a USB exfil case.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2020-04-18 14:23

## Timeline

A USBSTOR Disk entry for a SanDisk device with its serial, and a Portable Devices volume label EXFIL.

## How to spot it

- A USBSTOR Disk subkey with vendor, product and serial
- A Portable Devices entry with the volume label
- Serial ties the device across hosts
- Volume label and timing match the exfil window

## Questions

```
1. Keys?              USBSTOR and Windows Portable Devices
2. Device?             SanDisk Ultra
3. Serial?             4C530001250624117433
4. Label?              EXFIL
5. Technique?          USB exfil (device trace)
```

## MITRE ATT&CK

- T1052.001 Exfiltration over USB
- T1025 Data from Removable Media

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
