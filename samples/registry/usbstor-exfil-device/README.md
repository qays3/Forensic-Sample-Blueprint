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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 934f92aeb16733f4973fcf1093302d431de3ace447fe0f566b0481b4bb40559d.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A USBSTOR Disk subkey with vendor, product and serial

## MITRE ATT&CK

- T1052.001 Exfiltration over USB
- T1025 Data from Removable Media

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
