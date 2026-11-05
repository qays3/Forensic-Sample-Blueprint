# Covert Channel Cookies

## Scenario

Data is smuggled in HTTP cookies: long base64 blobs in request and response cookies carry the covert channel under cover of static asset requests.

Evidence: `covert-channel-cookies.pcap`  
Capture date: 2025-12-18

## Timeline

~12:05  GET /style.css with a large base64 cookie, response Set-Cookie carries data back.

## How to spot it

- Unusually long base64 cookie values
- Cookies on static asset requests
- Data in both request and response cookies
- Regular interval to one host

## Questions

```
1. Channel?           HTTP cookies
2. Cover?               static asset GETs
3. Encoding?            base64 in cookie
4. Both ways?            yes
5. Technique?          Covert channel
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `covert-channel-cookies.pcap`.
2. Open `covert-channel-cookies.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.cookie`  (Large base64 cookies). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r covert-channel-cookies.pcap -Y "http.cookie"`, and `tshark -r covert-channel-cookies.pcap -q -z conv,ip`.
5. Confirm against the answer: Unusually long base64 cookie values

## MITRE ATT&CK

- T1071.001 Web Protocols
- T1132.001 Standard Encoding

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
