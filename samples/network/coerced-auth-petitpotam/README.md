# coerced-auth-petitpotam

## Scenario
An attacker (10.10.1.66) calls the MS-EFSR (EfsRpcOpenFileRaw) method on the domain controller 10.10.2.10 over SMB, pointing it at an attacker UNC path. This coerces the DC to authenticate back to the attacker, leaking the DC machine account (DC01$) authentication (PetitPotam).

## Timeline (UTC+3)
```
12:20:00  Attacker calls EfsRpcOpenFileRaw on DC 10.10.2.10 via \pipe\lsarpc
12:20:00  The call points at \\10.10.1.66\share
12:20:01  DC 10.10.2.10 authenticates to the attacker with NTLMSSP as DC01$
```

## How to spot it
- An RPC call over SMB to the lsarpc or efsrpc pipe with a UNC path argument
- Right after, the called host authenticates back to the attacker
- The authenticating account is a machine account (ends with $)
- The attacker UNC path is where the coerced auth is sent

## Questions
```
1. Attacker IP?         10.10.1.66
2. Coerced host (DC)?   10.10.2.10
3. Method abused?       MS-EFSR EfsRpcOpenFileRaw
4. Account that leaked? DC01$ (machine account)
5. Technique name?      PetitPotam coercion
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 35bc4e4ada671117bc92b9dc628935715b1d3a0de5934c03465edd1ed586f4dc.zip -phidden`. You get `coerced-auth-petitpotam.pcap`.
2. Open `coerced-auth-petitpotam.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r coerced-auth-petitpotam.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r coerced-auth-petitpotam.pcap -q -z conv,ip`.
5. Confirm against the answer: An RPC call over SMB to the lsarpc or efsrpc pipe with a UNC path argument

## MITRE ATT&CK
- T1187 Forced Authentication

## Tools
Crafted MS-EFSR coercion and NTLMSSP reply on port 445 (scapy) over generated background traffic. Payloads are representative markers.


## Note
This pcap is a representative sample built with scapy: ports and markers are correct but the payload bytes are placeholders, not full protocol data. A faithful capture of this attack needs a real Windows domain controller, which this lab (Samba) cannot reproduce.

## Signature
Forensic Sample Blueprint | qayssarayra.com
