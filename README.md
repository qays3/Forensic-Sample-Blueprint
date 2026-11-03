# Forensic Sample Blueprint

## About

A library of attack samples for DFIR practice. Each sample is a real artifact made in an isolated lab: network captures, event logs, NTFS files, registry hives, memory images, disk images, cloud logs and more. Every sample comes with an answer key so you can check your findings.

Use it to practice investigations, test detection rules, train analysts, or build CTF challenges.

## What Each Sample Includes

* The sample files
* `answer-key.txt`: scenario story, ground truth timeline, MITRE ATT&CK technique IDs, tools and lab setup
* `hashes.sha256`: SHA256 hash of every file

## Safety

Files that hold live malware are zipped with the password `infected`. Open them only inside an isolated VM. No real user data is in any sample.

## Attack Catalog

The catalog below lists every attack this library aims to cover and which sample types capture it.

### Sample Codes

Every attack below lists the sample types that capture it, using these codes. Sections follow MITRE ATT&CK tactics, then platform and domain sections.

| Code | Sample type | Files |
| --- | --- | --- |
| PCAP | Network capture | .pcap, .pcapng, Zeek logs, NetFlow, Suricata eve.json, firewall and proxy logs |
| EVTX | Windows event logs | Security, System, Sysmon, PowerShell Operational, TaskScheduler, WMI Activity, RDP logs |
| MFT | NTFS artifacts | `$MFT`, `$UsnJrnl:$J`, `$LogFile`, `$I30`, `$Secure`, ADS |
| REG | Registry hives | SYSTEM, SOFTWARE, SAM, SECURITY, NTUSER.DAT, UsrClass.dat, Amcache.hve |
| EXEC | Execution artifacts | Prefetch, ShimCache, Amcache, SRUM, BAM, LNK, Jump Lists |
| MEM | Memory image | .raw, .mem, .vmem, hiberfil.sys, pagefile.sys, crash dumps, LSASS dumps |
| DISK | Disk image | .E01, .dd, .vmdk, .vhdx, AFF4, triage collections (KAPE, Velociraptor) |
| BROW | Browser artifacts | History, Cookies, Downloads, Login Data, cache, extensions |
| LNX | Linux and macOS artifacts | auth.log, syslog, journal, auditd, .bash\_history, cron, wtmp, btmp, unified logs |
| WEB | Web logs | Apache, Nginx, IIS, WAF, app logs |
| CLOUD | Cloud and identity logs | CloudTrail, VPC Flow Logs, Azure Activity, Entra ID sign in and audit, M365 UAL, GCP audit, Kubernetes audit |
| MAIL | Email | .eml, .msg, .pst, message trace, headers |
| FILE | Attack file sample | Documents, scripts, PE files, ISO, LNK, defanged payloads |
| EDR | EDR and SIEM exports | Process trees, alerts, Wazuh, Splunk, Elastic NDJSON |

### Reconnaissance

| Attack | Samples | Key evidence |
| --- | --- | --- |
| TCP SYN scan | PCAP | Many SYN to many ports, RST replies |
| TCP connect scan | PCAP, EVTX | Full handshakes, firewall 5156/5157 |
| UDP scan | PCAP | ICMP port unreachable replies |
| Xmas, NULL, FIN scans | PCAP | Odd TCP flag combos |
| ACK scan for firewall mapping | PCAP | ACK with no session |
| Idle (zombie) scan | PCAP | IP ID changes on third host |
| Ping sweep | PCAP | ICMP echo to whole subnet |
| ARP sweep | PCAP | ARP requests across subnet |
| Service and version detection | PCAP | Nmap probes, banner grabs |
| OS fingerprinting | PCAP | Odd TTL, window size, options |
| Nmap NSE script scans | PCAP, WEB | NSE user agent, script payloads |
| Masscan high rate scan | PCAP | Very high packet rate, fixed source port |
| Vulnerability scan (Nessus, OpenVAS, Nuclei) | PCAP, WEB | Thousands of probe URLs, scanner user agents |
| Web directory brute force (Gobuster, ffuf, Dirb) | WEB, PCAP | Burst of 404s from one IP |
| Web crawling and spidering | WEB | Fast sequential page hits |
| Subdomain brute force | PCAP | Many NXDOMAIN DNS answers |
| DNS zone transfer (AXFR) | PCAP | AXFR query over TCP 53 |
| DNS reverse lookup sweep | PCAP | Many PTR queries |
| SNMP walk | PCAP | SNMP GetNext with community strings |
| SMB share and null session enum | PCAP, EVTX | IPC$ access, 5140, 4624 anonymous |
| LDAP anonymous enum | PCAP | LDAP search with no bind |
| SMTP user enum (VRFY, EXPN, RCPT) | PCAP | Repeated VRFY commands |
| NetBIOS and LLMNR enum | PCAP | NBNS and LLMNR queries |
| Banner grabbing (FTP, SSH, Telnet) | PCAP | Short connects, no auth |
| WAF fingerprinting (wafw00f) | WEB | Attack strings to trigger blocks |
| Wireless recon (beacon and probe capture) | PCAP | 802.11 management frames |
| OSINT and phishing recon | BROW | Searches for staff, LinkedIn, email formats |
| Cloud bucket enumeration | CLOUD, PCAP | Many GET to bucket names, 404 and 403 |
| Email address validation via O365 | CLOUD | Sign in probes, user not found errors |

### Resource Development

Most of this stage happens on attacker infrastructure, so samples show its traces on the victim side.

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Lookalike domain registration | PCAP, MAIL | Typosquat domains in DNS and email links |
| Newly registered domain for C2 | PCAP | Young domain, cheap TLD, Let's Encrypt cert |
| Compromised legit site as staging | PCAP, WEB | Payload pulled from real but hacked site |
| Abuse of cloud storage for payloads | PCAP, BROW | Downloads from Dropbox, OneDrive, GitHub raw, Discord CDN |
| Code signing with stolen cert | FILE, DISK | Signed malware with revoked or odd cert |
| Malicious browser extension upload | BROW | New extension ID, wide permissions |
| Malicious npm or PyPI package | DISK, LNX, PCAP | Install scripts fetching payloads |
| Bulletproof VPS and redirectors | PCAP | Traffic to hosting ASNs known for abuse |

### Initial Access

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Spearphishing attachment (macro document) | MAIL, FILE, EVTX, EXEC | WINWORD spawning cmd or PowerShell |
| Spearphishing link (credential harvest) | MAIL, BROW, PCAP, CLOUD | Click, fake login page, then new sign in |
| HTML smuggling | MAIL, FILE, BROW, MFT | HTML attachment drops ZIP or ISO in Downloads |
| ISO, IMG, VHD container delivery | FILE, EVTX, MFT, REG | Mounted drive letter, LNK inside, no MOTW |
| Malicious LNK delivery | FILE, EXEC, EVTX | LNK target with hidden PowerShell |
| OneNote attachment payload | FILE, EVTX | ONENOTE spawning script host |
| Password protected ZIP with payload | MAIL, FILE | Password in email body |
| CHM, HTA, JS, VBS, WSF attachments | FILE, EVTX, EXEC | mshta, wscript, cscript execution |
| QR code phishing | MAIL, BROW | Image with URL, mobile sign in |
| Phishing over Teams or Slack | CLOUD, BROW | External chat message with link |
| Callback phishing (fake support) | EVTX, PCAP | Remote tool installed after call |
| Drive by download | BROW, PCAP, MFT | Exploit kit landing page, dropped file |
| Watering hole | BROW, PCAP | Injected script on trusted site |
| SEO poisoning fake installer | BROW, FILE, EXEC | Fake software site, signed loader |
| Malvertising | BROW, PCAP | Ad redirect chain to payload |
| Exploit public facing web app | WEB, PCAP, LNX, EVTX | Exploit request then webshell or shell |
| Exploit VPN or firewall appliance (Fortinet, Pulse, Citrix) | PCAP, WEB | Path traversal or RCE strings to appliance |
| Exploit Exchange (ProxyShell, ProxyLogon) | WEB, EVTX, DISK | Autodiscover abuse, aspx webshell |
| Log4Shell | WEB, PCAP | jndi ldap strings, LDAP callback |
| Exposed RDP brute force | EVTX, PCAP | 4625 storm then 4624 type 10 |
| Exposed SSH brute force | LNX, PCAP | Failed password lines then accepted |
| Valid stolen accounts | EVTX, CLOUD | Login from new IP, country, device |
| Password spraying | EVTX, CLOUD | One password, many users, 4771 or 50126 |
| Credential stuffing | WEB, CLOUD | Leaked combos against login page |
| MFA fatigue (push bombing) | CLOUD | Many MFA denied then approved |
| Adversary in the middle phishing (Evilginx) | CLOUD, BROW, PCAP | Session cookie replay from new IP |
| Device code phishing | CLOUD | Device code flow sign in |
| OAuth consent phishing | CLOUD | Consent to unknown app with mail scopes |
| Supply chain compromised update | DISK, PCAP, EVTX | Trusted updater spawning odd child |
| Malicious dependency install | LNX, DISK, PCAP | postinstall script runs curl |
| Trusted third party (MSP) abuse | EVTX, PCAP | RMM tool from vendor runs commands |
| Infected USB drop | REG, EVTX, EXEC, MFT | USBSTOR entry, autorun, LNK execution |
| Rubber Ducky or BadUSB HID attack | REG, EVTX | New keyboard device, fast typed commands |
| Rogue wireless access point | PCAP | Evil twin beacons, deauth frames |
| Exposed database or service (Redis, Elastic, Mongo) | PCAP, LNX | Unauth commands from internet IP |
| Exposed Docker API | PCAP, LNX | Container create from remote IP |
| Exposed Kubernetes dashboard or API | CLOUD | Anonymous API calls |
| Insider with legit access | EVTX, REG, CLOUD | Normal user, abnormal volume |

### Execution

| Attack | Samples | Key evidence |
| --- | --- | --- |
| PowerShell encoded command | EVTX, MEM, EDR | 4104 script block, 4688 with -enc |
| PowerShell download cradle (IEX, DownloadString) | EVTX, PCAP | Net.WebClient, Invoke-WebRequest |
| PowerShell without powershell.exe (Unmanaged) | EVTX, MEM | System.Management.Automation.dll in odd process |
| PowerShell v2 downgrade | EVTX | EngineVersion 2.0 in 400 event |
| AMSI bypass | EVTX, MEM | amsiInitFailed, patched AmsiScanBuffer |
| cmd.exe batch scripts | EVTX, EXEC, MFT | .bat in temp, cmd parent chain |
| WScript and CScript (VBS, JS) | EVTX, EXEC | wscript.exe with script in user path |
| mshta HTA execution | EVTX, EXEC, PCAP | mshta with URL argument |
| rundll32 abuse | EVTX, EXEC | rundll32 with DLL in temp or JS |
| regsvr32 Squiblydoo | EVTX, PCAP | regsvr32 /i:http scrobj.dll |
| msiexec remote install | EVTX, PCAP | msiexec /q /i http |
| InstallUtil, MSBuild, RegAsm, RegSvcs | EVTX, EXEC | .NET signed binary running user code |
| certutil download and decode | EVTX, MFT | certutil -urlcache -f, -decode |
| bitsadmin download | EVTX | BITS Client 59, 60 events |
| WMIC process call create | EVTX, PCAP | WmiPrvSE spawning cmd |
| WMIC XSL remote script | EVTX, PCAP | wmic /format:http |
| Scheduled task one time run | EVTX, DISK | 4698, Tasks folder XML |
| Service execution (sc create) | EVTX, REG | 7045, new Services key |
| Native API and direct syscalls | MEM, EDR | Unbacked memory calling syscalls |
| Shared modules (LoadLibrary of payload) | MEM, EVTX | Sysmon 7 unsigned DLL load |
| User execution of malicious file | EXEC, MFT, EVTX | Explorer parent, Downloads path |
| Office macro (VBA) | FILE, EVTX | Office child process, VBA in file |
| Excel 4.0 (XLM) macros | FILE, EVTX | Hidden macro sheets |
| DDE in Office | FILE, EVTX | DDEAUTO field |
| Template injection (remote .dotm) | FILE, PCAP | settings.xml.rels to remote URL |
| Follina (msdt) | FILE, EVTX | msdt with ms-msdt URL |
| Container admin command (docker exec) | LNX | Docker daemon logs |
| Cloud run command (SSM, Azure Run Command) | CLOUD, EVTX | SendCommand or RunCommand events |
| Python, Node, Perl inline payloads | LNX, EVTX | Interpreter with -c argument |
| Unix shell reverse shell | LNX, PCAP | bash -i to /dev/tcp |
| AutoHotKey and AutoIT scripts | FILE, EXEC | Compiled AutoIT binaries |
| JavaScript in LNK or Windows Script Components | FILE, EVTX | .sct, .wsc files |
| Inter process via COM (MMC20, ShellWindows) | EVTX, PCAP | DCOM spawning mmc or explorer child |
| Exploitation for client execution (browser, PDF reader) | MEM, EVTX | Browser or reader spawning shell |

### Persistence

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Run and RunOnce keys (HKCU, HKLM) | REG, EVTX | Sysmon 13 registry set |
| Startup folder | MFT, DISK, EXEC | New file in Startup path |
| Scheduled task | EVTX, DISK, REG | 4698, TaskCache tree keys |
| Hidden scheduled task (SD deleted) | REG, DISK | Task with no SD value |
| New service | EVTX, REG | 7045, 4697 |
| Modified existing service ImagePath | REG, EVTX | Changed path on known service |
| Service DLL hijack (svchost ServiceDll) | REG, EVTX | Parameters ServiceDll changed |
| WMI event subscription | EVTX, MEM, DISK | WMI 5861, OBJECTS.DATA entries |
| Winlogon Userinit and Shell | REG | Extra value after userinit.exe |
| Winlogon Notify and AppInit\_DLLs | REG | DLL path in AppInit |
| Image File Execution Options debugger | REG | Debugger value on sethc or utilman |
| Sticky keys and Utilman backdoor | MFT, REG, EVTX | sethc.exe replaced by cmd.exe |
| Silent Process Exit monitor | REG | MonitorProcess value |
| COM hijacking | REG | HKCU CLSID InprocServer32 override |
| DLL search order hijack | MFT, EVTX | Sysmon 7 DLL from app folder |
| DLL side loading | MFT, EVTX, EXEC | Signed EXE plus malicious DLL in temp |
| Phantom DLL hijack | MFT, EVTX | Missing DLL placed by attacker |
| Accessibility features abuse | REG, MFT | osk, magnify, narrator replaced |
| Office startup (Normal.dotm, XLSTART, add ins) | MFT, REG | Modified template or new add in |
| Office test registry key | REG | Office test Special Perf key |
| Outlook rules, forms, home page | REG, CLOUD | Outlook rule launching app |
| Browser extension | BROW, REG | Force installed extension |
| Local account creation | EVTX, REG | 4720, 4732 |
| Domain account creation | EVTX | 4720 on DC, added to groups |
| Hidden account ($ name) | EVTX, REG | Username ending with $ |
| AdminSDHolder change | EVTX | 5136 on AdminSDHolder |
| SID History injection | EVTX | 4765, 4766 |
| Golden Ticket | EVTX, PCAP, MEM | TGS with no matching TGT, odd lifetime |
| Skeleton key | MEM, EVTX | Patched LSASS on DC |
| Malicious SSP (mimilib) | REG, MFT | Security Packages value changed |
| Password filter DLL | REG, MFT | Notification Packages changed |
| Netsh helper DLL | REG | Netsh key new DLL |
| Port monitor and print processor | REG, MFT | Monitors key DLL in System32 |
| Time provider DLL | REG | W32Time TimeProviders change |
| LSA authentication package | REG | Authentication Packages change |
| BITS job persistence | EVTX, DISK | BITS 3, 59 events, qmgr database |
| Screensaver hijack | REG | SCRNSAVE.EXE path |
| Application shimming (SDB) | REG, MFT | Custom sdb installed |
| Boot or logon script (UserInitMprLogonScript) | REG | Environment key value |
| GPO logon script or scheduled task | EVTX, DISK | SYSVOL changes, 5136 |
| Webshell | WEB, MFT, DISK | New aspx, php, jsp in web root |
| IIS module backdoor | DISK, EVTX | New native module in applicationHost.config |
| Exchange transport agent | DISK, EVTX | Unknown transport agent |
| Bootkit and MBR or VBR change | DISK | Modified boot sectors |
| UEFI implant | DISK | Changed ESP files |
| Remote access tool install (AnyDesk, ScreenConnect) | EVTX, REG, PCAP | New service and outbound to vendor |
| Linux cron job | LNX | New crontab line |
| Linux systemd service or timer | LNX | New unit file |
| SSH authorized\_keys | LNX | New key line, file mtime |
| bashrc, profile modification | LNX | Added reverse shell line |
| LD\_PRELOAD and ld.so.preload | LNX | New entry in ld.so.preload |
| PAM backdoor | LNX | Modified pam\_unix.so |
| Kernel module rootkit | LNX, MEM | Hidden module in memory |
| macOS LaunchAgent and LaunchDaemon | LNX | New plist in LaunchAgents |
| Cloud access key creation | CLOUD | CreateAccessKey for other user |
| Cloud backdoor role or app registration | CLOUD | New service principal secret |
| Mailbox forwarding rule | CLOUD | New-InboxRule to external |

### Privilege Escalation

| Attack | Samples | Key evidence |
| --- | --- | --- |
| UAC bypass fodhelper | REG, EVTX | ms-settings shell open command key |
| UAC bypass eventvwr | REG, EVTX | mscfile shell open command |
| UAC bypass sdclt, computerdefaults, CMSTP | REG, EVTX | Auto elevate binary spawning cmd |
| Token impersonation (Potato family) | EVTX, MEM | Service account spawning SYSTEM shell |
| PrintSpoofer and named pipe impersonation | EVTX, MEM | Spooler pipe abuse |
| SeDebugPrivilege abuse | EVTX, MEM | 4703 enable, handle to SYSTEM process |
| SeBackupPrivilege abuse | EVTX, MFT | SAM and SYSTEM read via backup API |
| Unquoted service path | REG, MFT, EVTX | Program.exe in C root |
| Weak service permissions | EVTX, REG | sc config binPath change |
| Weak folder permissions on service binary | MFT, EVTX | Service EXE replaced |
| AlwaysInstallElevated | REG, EVTX | Both keys set, msiexec as SYSTEM |
| Vulnerable driver (BYOVD) | EVTX, MFT, MEM | Sysmon 6 driver load, known bad hash |
| Kernel exploit (local) | MEM, EVTX, DISK | Exploit binary, crash dumps |
| PrintNightmare | EVTX, MFT | New driver in spool folder, 316 event |
| HiveNightmare (SeriousSAM) | MFT, EVTX | Shadow copy hive read by user |
| Stored credentials in files (unattend.xml, GPP) | DISK, PCAP | cpassword in SYSVOL |
| Kerberos delegation abuse (unconstrained, RBCD) | EVTX, PCAP | 4742 msDS-AllowedToActOnBehalf change |
| AD CS abuse (ESC1 to ESC8) | EVTX, PCAP | 4886, 4887 cert with other user SAN |
| ZeroLogon | EVTX, PCAP | Netlogon calls with zero challenge, 4742 |
| noPac (sAMAccountName spoof) | EVTX | 4741 then rename to DC name |
| GPO abuse for privilege | EVTX, DISK | 5136 on GPO, new task in SYSVOL |
| DACL abuse (GenericAll, WriteDACL) | EVTX | 5136, 4662 |
| Linux sudo misconfig | LNX | sudo log of GTFOBins command |
| Linux SUID binary abuse | LNX | find with exec as root |
| Linux capabilities abuse | LNX | cap\_setuid on python |
| Linux kernel exploit (Dirty Pipe, PwnKit) | LNX, MEM | pkexec crash, odd binaries in tmp |
| Writable cron or PATH hijack | LNX | Root cron script modified |
| Docker group or socket escape | LNX | docker run with host mount |
| Container escape (privileged container) | LNX, CLOUD | Host namespace access |
| Cloud IAM escalation (PassRole, attach policy) | CLOUD | AttachUserPolicy, PassRole calls |
| Azure role assignment escalation | CLOUD | New Owner role assignment |
| Entra ID privileged role add | CLOUD | Add member to role Global Admin |
| Access token manipulation (runas, make token) | EVTX, MEM | 4624 type 9, 4648 |

### Defense Evasion

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Timestomping | MFT | `$SI` earlier than `$FN`, zero nanoseconds |
| Clear Windows event logs | EVTX | 1102, 104 |
| Delete single log records (EventCleaner, DanderSpritz) | EVTX, MEM | Gaps in record IDs |
| Stop EventLog service thread (Phant0m) | EVTX, MEM | Silence with service still running |
| Disable Defender (registry, Set-MpPreference) | REG, EVTX | 5001, 5007, DisableAntiSpyware |
| Defender exclusion added | REG, EVTX | Exclusions Paths key, 5007 |
| Tamper or kill EDR (BYOVD EDR killer) | EVTX, MEM | Driver load then EDR stop |
| Disable Sysmon or unload driver | EVTX | Sysmon 4 state change, fltmc unload |
| Disable firewall | REG, EVTX | 2003, netsh advfirewall off |
| Disable or clear PowerShell logging | REG, EVTX | ScriptBlockLogging set to 0 |
| ETW patching | MEM | Patched EtwEventWrite |
| AMSI patching | MEM | Patched AmsiScanBuffer |
| File deletion of tools | MFT, EXEC | `$UsnJrnl` delete records, Prefetch left |
| Secure delete (SDelete, cipher /w) | MFT | Renamed to AAAA names before delete |
| `$UsnJrnl` deletion | MFT, EVTX | fsutil usn deletejournal |
| Volume shadow copy deletion | EVTX | vssadmin delete, wmic shadowcopy delete |
| Prefetch disabled or deleted | REG, MFT | EnablePrefetcher 0 |
| Hidden files and attributes | MFT | Hidden, system flags, attrib +h +s |
| Alternate data streams | MFT, FILE | Payload in named stream |
| Masquerading (svch0st, wrong path) | EXEC, EVTX | Known name in odd folder |
| Right to left override filename | MFT, MAIL | U+202E in name |
| Double extension | MAIL, MFT | invoice.pdf.exe |
| Renamed system utilities | EVTX, EXEC | OriginalFileName mismatch |
| Mark of the Web bypass | MFT, FILE | Missing Zone.Identifier |
| Obfuscated or encoded scripts | FILE, EVTX | Base64, char codes, string splits |
| Packed binaries (UPX, custom) | FILE, MEM | High entropy, few imports |
| Process injection classic (VirtualAllocEx, CreateRemoteThread) | MEM, EVTX | Sysmon 8, RWX region |
| Process hollowing | MEM | Image path mismatch, unmapped section |
| Process doppelganging and herpaderping | MEM, MFT | TxF use, file content mismatch |
| APC injection and Early Bird | MEM | Queued APC in suspended thread |
| Thread hijacking | MEM | Thread start in odd region |
| Reflective DLL loading | MEM | PE in memory not on disk |
| Module stomping | MEM | Legit DLL with changed code |
| Parent PID spoofing | EVTX, MEM | Parent not matching creator |
| Command line spoofing | MEM, EVTX | PEB args differ from 4688 |
| Signed binary proxy (LOLBins) | EVTX, EXEC | rundll32, mshta, regsvr32 chains |
| Trusted developer utilities (MSBuild) | EVTX | MSBuild with inline task |
| Rootkit (user mode hooks) | MEM | IAT, inline hooks |
| Rootkit (kernel DKOM) | MEM | Unlinked EPROCESS |
| Indicator removal on host (history clear) | LNX, REG | Empty bash\_history, unset HISTFILE |
| Log tampering on Linux | LNX | wtmp edits, gaps in auth.log |
| Disable auditd or SELinux | LNX | auditctl -e 0, setenforce 0 |
| Hide artifacts in VM or sandbox evasion | MEM, FILE | Checks for VM drivers |
| Domain policy change (GPO weaken) | EVTX | 5136 security policy |
| Code signing abuse | FILE, EVTX | Signed malware |
| Impair cloud logging | CLOUD | StopLogging, DeleteTrail, diagnostic setting delete |
| Use of alternate auth material | EVTX, CLOUD | Token reuse with no password events |
| Fileless registry stored payload | REG, MEM | Large binary value in registry |
| Steganography in images | FILE, PCAP | Payload hidden in PNG |
| Virtualization (run VM on victim) | DISK, EVTX | VirtualBox install, VM files |

### Credential Access

| Attack | Samples | Key evidence |
| --- | --- | --- |
| LSASS dump with Mimikatz | EVTX, MEM | Sysmon 10 GrantedAccess 0x1010 |
| LSASS dump with procdump or comsvcs MiniDump | EVTX, MFT | rundll32 comsvcs MiniDump, .dmp file |
| LSASS dump with Task Manager | EVTX, MFT | lsass.DMP in temp |
| Nanodump and silent process exit dump | REG, EVTX | SilentProcessExit LocalDumpFolder |
| SAM and SYSTEM hive dump | EVTX, MFT | reg save HKLM\\SAM |
| LSA secrets and cached creds dump | REG, MEM | SECURITY hive access |
| NTDS.dit theft (ntdsutil IFM, vssadmin) | EVTX, MFT | ntdsutil, shadow copy create, 325 ESENT |
| DCSync | EVTX, PCAP | 4662 with replication GUIDs from non DC |
| DCShadow | EVTX, PCAP | New DC object registered then removed |
| Kerberoasting | EVTX, PCAP | 4769 RC4 (0x17) for SPN accounts |
| AS-REP roasting | EVTX, PCAP | 4768 with no preauth |
| Kerberos brute force | EVTX, PCAP | 4771 failures |
| Password spraying (LDAP, SMB, Kerberos) | EVTX, PCAP | One password, many accounts |
| Brute force local | EVTX | 4625 burst |
| LLMNR and NBT-NS poisoning (Responder) | PCAP, EVTX | Spoofed name replies, NTLM hash capture |
| WPAD spoofing | PCAP | Rogue wpad.dat |
| NTLM relay (ntlmrelayx) | PCAP, EVTX | 4624 from relay IP, signing off |
| Coerced auth (PetitPotam, PrinterBug, DFSCoerce) | PCAP, EVTX | EFSRPC, MS-RPRN calls to attacker |
| Shadow credentials | EVTX | msDS-KeyCredentialLink change |
| Pass the Hash | EVTX | 4624 type 3 NTLM, logon process NtLmSsp |
| Pass the Ticket | EVTX, MEM | Ticket use with no TGT request |
| Overpass the Hash | EVTX | 4768 RC4 from workstation |
| Silver Ticket | EVTX | Service logon with no 4769 |
| Diamond and Sapphire tickets | EVTX, PCAP | Modified real TGT |
| Browser saved passwords theft | BROW, EVTX, MFT | Access to Login Data and Local State |
| Browser cookie theft | BROW, MFT | Cookies file copied |
| DPAPI master key abuse | MFT, MEM | Protect folder access |
| Credential Manager and vault dump | EVTX | 5379, vaultcmd |
| Keylogging | MEM, FILE, MFT | SetWindowsHookEx, log file |
| Clipboard theft | MEM | Clipboard API polling |
| Fake credential prompt | EVTX, MEM | PowerShell Get-Credential popup |
| Credentials in files and scripts | DISK | Passwords in ps1, config, xml |
| Credentials in registry (Winlogon AutoLogon) | REG | DefaultPassword value |
| KeePass and password manager theft | MEM, MFT | KeePass process dump, kdbx copy |
| WiFi password dump | EVTX | netsh wlan show profile key=clear |
| Linux /etc/shadow theft | LNX | Read of shadow, unshadow |
| SSH private key theft | LNX | Read of id\_rsa |
| Linux memory credential dump (mimipenguin) | LNX, MEM | Access to process memory |
| Network sniffing of cleartext creds | PCAP | FTP, Telnet, HTTP basic, POP3 creds |
| Cloud instance metadata theft (IMDS) | CLOUD, PCAP, WEB | 169.254.169.254 requests, SSRF |
| Cloud keys in code or env files | DISK, LNX | .aws credentials read |
| Session token theft (Entra PRT, cookies) | CLOUD, MEM | Token replay from new device |
| Steal app access token | CLOUD | Token use from odd IP |
| MFA interception or SIM swap | CLOUD | MFA method change |
| Forced auth via SCF or LNK in share | PCAP, MFT | SMB auth to attacker from icon path |

### Discovery

| Attack | Samples | Key evidence |
| --- | --- | --- |
| System info (systeminfo, hostname, ver) | EVTX, EXEC | Burst of recon commands |
| User and group enum (net user, net group) | EVTX, PCAP | net.exe and SAMR calls |
| Domain admins enum | EVTX, PCAP | net group domain admins /domain |
| Domain trust enum (nltest) | EVTX | nltest /domain\_trusts |
| BloodHound and SharpHound collection | EVTX, PCAP, MFT | Huge LDAP query burst, zip output file |
| AdFind | EVTX, EXEC | adfind.exe with filters |
| PowerView | EVTX | 4104 with Get-Domain cmdlets |
| LDAP recon queries | PCAP, EVTX | 1644 expensive queries |
| Network share enum (net view, SharpShares) | EVTX, PCAP | 5140 across many hosts |
| Internal port scan | PCAP, EVTX | One host hitting many internal IPs |
| Network config (ipconfig, route, arp) | EVTX | Recon command chain |
| Network connections (netstat) | EVTX | netstat -ano |
| Process discovery (tasklist) | EVTX | tasklist /v |
| Security software discovery | EVTX, REG | WMI AntiVirusProduct query |
| Installed software enum | EVTX, REG | Uninstall keys read |
| Remote system discovery (ping sweep internal) | PCAP | ICMP to subnet |
| Account policy discovery | EVTX | net accounts |
| File and directory discovery | EVTX, REG | dir /s, tree, ShellBags |
| Query registry | EVTX | reg query of keys |
| Peripheral device discovery | EVTX | WMI Win32\_PnPEntity |
| System time discovery | EVTX | net time, w32tm |
| Virtualization and sandbox check | MEM, FILE | VM artifact queries |
| Browser bookmark and history discovery | BROW | Reads of bookmarks file |
| Password policy and GPO discovery | EVTX, PCAP | SYSVOL reads |
| Linux recon (id, uname, ps, ss, cat /etc/passwd) | LNX | Commands in history or auditd |
| Cloud recon (enumerate IAM, buckets, VMs) | CLOUD | Burst of List and Describe calls |
| Azure AD recon (AzureHound, ROADtools) | CLOUD | Graph API burst from one token |
| Kubernetes recon (kubectl get all) | CLOUD | Audit log list calls |

### Lateral Movement

| Attack | Samples | Key evidence |
| --- | --- | --- |
| PsExec (Sysinternals) | EVTX, REG, PCAP, EXEC | 7045 PSEXESVC, ADMIN$ write, 4624 type 3 |
| Impacket psexec, smbexec, atexec | EVTX, PCAP | Random service names, output to ADMIN$ |
| Impacket wmiexec | EVTX, PCAP | WmiPrvSE cmd with output to ADMIN$ |
| Impacket dcomexec | EVTX, PCAP | MMC20 DCOM spawning cmd |
| WMI remote process | EVTX, PCAP | 4624 type 3, WMI activity events |
| WinRM and PowerShell remoting | EVTX, PCAP | wsmprovhost.exe, 5985 traffic, 91 and 168 events |
| RDP | EVTX, PCAP, REG | 4624 type 10, 1149, 21, 22, 25 |
| RDP hijack (tscon) | EVTX | tscon as SYSTEM |
| SSH lateral (Linux) | LNX, PCAP | Accepted publickey from internal host |
| SMB admin share copy | EVTX, PCAP, MFT | 5145 C$ writes |
| Remote scheduled task | EVTX, PCAP | 4698 from remote, ATSVC pipe |
| Remote service creation (sc \\host) | EVTX, PCAP | 7045 from SVCCTL pipe |
| Remote registry changes | EVTX, PCAP | winreg pipe use |
| DCOM (ShellWindows, ShellBrowserWindow) | EVTX, PCAP | explorer child from DCOM |
| Pass the Hash lateral | EVTX | 4624 NTLM type 3 with admin |
| Pass the Ticket lateral | EVTX | Kerberos logon with no TGT request |
| SCCM or software deployment abuse | EVTX, DISK | Package pushed to many hosts |
| GPO deployment of malware | EVTX, DISK | SYSVOL script then many hosts run it |
| RMM tools (AnyDesk, TeamViewer, Atera) | EVTX, PCAP, DISK | Install and connection logs |
| VNC | PCAP, EVTX | 5900 traffic |
| Cobalt Strike jump and remote exec | EVTX, PCAP, MEM | Random service, named pipes like msagent |
| Internal spearphishing | MAIL, CLOUD | Internal sender with malicious link |
| Taint shared content (LNK in share) | MFT, PCAP | New LNK or EXE on file server |
| Replication through removable media | REG, MFT | USB with autorun copy |
| Cloud lateral (assume role, VM serial console) | CLOUD | AssumeRole chain across accounts |
| Container to host or pod to pod | CLOUD, LNX | kubectl exec into many pods |
| SSH hijack (agent forwarding) | LNX | SSH\_AUTH\_SOCK reuse |
| Exploitation of remote services (EternalBlue) | PCAP, MEM | SMBv1 trans2 exploit pattern |

### Collection

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Archive with 7zip, WinRAR, zip | EVTX, MFT, EXEC | Big archive in temp or ProgramData |
| Password protected archive | EVTX, FILE | rar a -hp |
| Local file staging | MFT | Many copies into one folder |
| Remote staging on file server | MFT, PCAP | Large SMB reads into one host |
| Data from network shares | EVTX, PCAP | 5145 burst on many files |
| Data from SharePoint and OneDrive | CLOUD | FileDownloaded burst in UAL |
| Mailbox collection (local PST export) | EVTX, MFT | New PST file |
| Mailbox collection (cloud, eDiscovery, Graph) | CLOUD | MailItemsAccessed, New-ComplianceSearch |
| Screen capture | MEM, MFT | Screenshot files, GDI capture |
| Audio and video capture | REG, MEM | CapabilityAccessManager mic and webcam keys |
| Keylogging output | MFT | Log file growth |
| Clipboard data | MEM | Clipboard polling |
| Database dump | LNX, EVTX, PCAP | mysqldump, sqlcmd export |
| Code repository clone | CLOUD, PCAP | git clone of many repos |
| Data from cloud storage (S3 sync) | CLOUD | GetObject burst |
| Browser data collection | BROW, MFT | Copy of browser profile |
| Removable media data copy | REG, MFT, EXEC | USB serial, LNK pointing to E drive |
| Automated collection scripts | EVTX | PowerShell Get-ChildItem by extension |

### Command and Control

| Attack | Samples | Key evidence |
| --- | --- | --- |
| HTTP beaconing | PCAP, MEM, EDR | Regular interval, same size requests |
| HTTPS beaconing | PCAP | JA3 and JA4 hash, self signed cert |
| Cobalt Strike default profile | PCAP, MEM | Known URIs, beacon config in memory |
| Sliver, Havoc, Mythic, Brute Ratel | PCAP, MEM | Framework specific URIs and certs |
| Metasploit Meterpreter | PCAP, MEM | Port 4444, reflective DLL in memory |
| Empire and PowerShell C2 | PCAP, EVTX | Base64 cookies, 4104 stagers |
| DNS C2 and tunneling (dnscat2, iodine) | PCAP | Long random subdomains, TXT records |
| DNS over HTTPS C2 | PCAP | Traffic to DoH resolvers from odd process |
| ICMP tunneling | PCAP | Large or patterned ICMP payloads |
| Domain fronting | PCAP | SNI differs from Host header |
| CDN and cloud fronted C2 | PCAP | Beacons to CloudFront, Azure edge |
| C2 over legit web services (Telegram, Discord, Slack, GitHub) | PCAP, MEM | API calls from non browser process |
| C2 over email | MAIL, PCAP | IMAP polling by malware |
| SMB named pipe peer to peer C2 | PCAP, MEM | Pipe names between internal hosts |
| Reverse SSH tunnel | PCAP, LNX | ssh -R to external host |
| Proxy and port forward (chisel, ligolo, plink) | PCAP, EVTX, MFT | New binary and long lived connection |
| SOCKS proxy through compromised host | PCAP | Many internal connections from one host |
| Tor C2 | PCAP | Tor node IPs, tor.exe |
| Ngrok and tunneling services | PCAP | ngrok.io domains |
| Non standard port | PCAP | HTTP over 8443, 53 non DNS |
| Protocol impersonation | PCAP | Fake TLS or HTTP |
| Fast flux and DGA domains | PCAP | Many random domain lookups, NXDOMAIN |
| Ingress tool transfer | PCAP, MFT, EVTX | Download then new file and execution |
| Remote access software as C2 | PCAP, EVTX | AnyDesk, ScreenConnect sessions |
| WebSocket C2 | PCAP | Upgrade headers to odd host |
| Covert channel in headers or cookies | PCAP | Encoded data in cookies |

### Exfiltration

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Exfil over C2 channel | PCAP, EXEC | Large upload on beacon channel, SRUM bytes sent |
| Exfil over HTTP POST | PCAP, WEB | Big POST bodies |
| Rclone to cloud (Mega, S3, Dropbox) | EVTX, PCAP, MFT | rclone.conf, rclone.exe in Prefetch |
| MEGAsync, WinSCP, FileZilla | EXEC, REG, PCAP | Tool install and big uploads |
| FTP and SFTP exfil | PCAP, LNX | STOR commands |
| DNS exfil | PCAP | Encoded data in queries |
| ICMP exfil | PCAP | Data in echo payloads |
| Email exfil | MAIL, CLOUD | Attachments to personal address |
| Exfil to webhook or paste site | PCAP, BROW | Pastebin, webhook.site posts |
| Exfil to GitHub or code repo | PCAP | git push to new repo |
| USB exfil | REG, MFT, EXEC, EVTX | USB serial, LNK to removable drive, 6416 |
| Bluetooth exfil | REG | Bluetooth device pairing keys |
| Printing documents | EVTX, DISK | PrintService 307 |
| Cloud storage share link | CLOUD | AnonymousLinkCreated |
| Cloud snapshot share to other account | CLOUD | ModifySnapshotAttribute, CreateImage |
| S3 bucket made public | CLOUD | PutBucketPolicy with public read |
| Database export over network | PCAP | Big SQL result sets |
| Scheduled or throttled exfil | PCAP | Small chunks at fixed times |
| Exfil via steganography | PCAP, FILE | Images uploaded with hidden data |
| Insider personal email upload | BROW, PCAP | Gmail upload of company files |

### Impact

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Ransomware file encryption | MFT, DISK, EVTX, MEM | Mass rename in `$UsnJrnl`, ransom note |
| Ransomware on ESXi | LNX, DISK | esxcli vm kill, encrypted vmdk |
| Ransomware via GPO across domain | EVTX, DISK | Same task on all hosts |
| Inhibit recovery (vssadmin, wbadmin, bcdedit) | EVTX | Delete shadows, recovery disabled |
| Backup deletion | EVTX, CLOUD | Backup job delete, vault delete |
| Disk wipe (wiper malware) | DISK, EVTX | Overwritten MBR and files |
| MBR overwrite | DISK | Changed sector 0 |
| Data destruction (cipher, sdelete, rm -rf) | MFT, LNX | Mass delete records |
| Data manipulation | DISK, LNX | Changed DB records or files |
| Defacement | WEB, MFT | index page changed |
| Service stop (databases, AV before encryption) | EVTX | 7036 stops, net stop burst |
| Account lockout or removal | EVTX, CLOUD | 4740, 4726 mass |
| Password change of admins | EVTX, CLOUD | 4724 burst |
| System shutdown and reboot | EVTX | 1074, 6006 |
| Resource hijacking (cryptomining) | LNX, PCAP, EVTX | xmrig, stratum protocol, high CPU |
| Cloud cryptomining | CLOUD | RunInstances of GPU types in new regions |
| DoS flood (SYN, UDP, HTTP) | PCAP, WEB | High rate from many IPs |
| Application layer DoS (Slowloris) | PCAP, WEB | Many open slow connections |
| Amplification attack (DNS, NTP, memcached) | PCAP | Large replies to spoofed IP |
| Firmware corruption | DISK | Modified firmware images |
| Cloud resource deletion | CLOUD | DeleteBucket, DeleteDBInstance burst |
| Financial theft (BEC payment change) | MAIL, CLOUD | Bank change request thread |
| Extortion leak site prep | PCAP | Upload of stolen data |

### Linux and macOS Attacks

Linux items already listed in earlier sections are not repeated here.

| Attack | Samples | Key evidence |
| --- | --- | --- |
| Webshell on Linux web server | WEB, LNX, DISK | www-data spawning bash |
| Reverse shell (bash, nc, python, socat) | LNX, PCAP | Outbound shell to odd port |
| Bind shell | LNX, PCAP | New listener on high port |
| Cryptominer dropper (curl pipe sh) | LNX, PCAP | curl or wget to /tmp then chmod +x |
| Malware in /tmp or /dev/shm | LNX, DISK | Executable in memory backed folders |
| Deleted running binary | LNX, MEM | /proc/pid/exe shows deleted |
| Fileless memfd\_create execution | LNX, MEM | memfd: in process maps |
| Process hiding with libprocesshider | LNX, MEM | ld.so.preload library |
| Diamorphine and Reptile rootkits | MEM, LNX | Hidden module, hooked syscalls |
| eBPF malware | MEM, LNX | Unknown loaded BPF programs |
| SSH backdoored binary | DISK, LNX | Changed sshd hash |
| SSH brute force botnet (Mirai style) | LNX, PCAP | Telnet and SSH brute force then busybox |
| Docker container breakout | LNX, DISK | Host files written from container |
| Malicious Docker image | DISK, LNX | Image with miner layer |
| Auditd evasion | LNX | Rules removed |
| journalctl vacuum and log wipe | LNX | Short journal, gaps |
| Shell history tampering | LNX | HISTSIZE 0, history -c |
| Timestomp with touch | LNX, DISK | ctime newer than mtime |
| sudo token reuse | LNX | sudo with no password prompt |
| Exploit of web app to RCE (e.g. CMS plugin) | WEB, LNX | POST to plugin path then shell |
| macOS malicious LaunchAgent | LNX, DISK | New plist |
| macOS Gatekeeper bypass | LNX | Missing quarantine attribute |
| macOS AppleScript and osascript abuse | LNX | osascript fake password prompt |
| macOS TCC database tamper | DISK | TCC.db changes |
| macOS keychain theft | LNX, DISK | security dump-keychain |
| macOS infostealer (Atomic, Poseidon) | FILE, LNX, PCAP | DMG install, data posted out |

### Cloud and Identity Attacks

All of these produce CLOUD samples. Extra sample types are noted where they help.

| Platform | Attack | Key evidence |
| --- | --- | --- |
| AWS | Leaked access key used from new IP | GetCallerIdentity first, then recon |
| AWS | IAM user or key creation for persistence | CreateUser, CreateAccessKey |
| AWS | Privilege escalation via policy attach | AttachUserPolicy, PutUserPolicy |
| AWS | PassRole to EC2 or Lambda | RunInstances or CreateFunction with admin role |
| AWS | Assume role chain | AssumeRole across accounts |
| AWS | CloudTrail disable or delete | StopLogging, DeleteTrail, PutEventSelectors |
| AWS | GuardDuty disable | DeleteDetector |
| AWS | S3 data theft | ListObjects then GetObject burst |
| AWS | S3 ransomware (encrypt with attacker KMS) | CopyObject with new KMS key |
| AWS | EBS snapshot share | ModifySnapshotAttribute to other account |
| AWS | Security group opened to world | AuthorizeSecurityGroupIngress 0.0.0.0/0 |
| AWS | EC2 IMDS credential theft via SSRF | Role creds used outside instance (plus WEB) |
| AWS | SSM SendCommand for execution | SendCommand on many instances |
| AWS | Lambda backdoor | UpdateFunctionCode |
| AWS | Console login without MFA | ConsoleLogin MFAUsed No |
| AWS | Cryptomining instances | RunInstances in unused regions |
| Azure | Role assignment escalation | Microsoft.Authorization roleAssignments write |
| Azure | VM Run Command | runCommand action |
| Azure | Storage account key listing and SAS abuse | listKeys, SAS token use |
| Azure | Key Vault secret theft | SecretGet burst |
| Azure | Diagnostic settings delete | diagnosticSettings delete |
| Azure | Managed identity token abuse | Token request from VM then API calls |
| Entra ID | Password spray | 50126 errors across users |
| Entra ID | MFA fatigue | Many MFA denied, then success |
| Entra ID | AiTM session hijack | Same session ID from two IPs |
| Entra ID | Impossible travel | Logins from far apart places in short time |
| Entra ID | Device code phishing | Sign in with deviceCode protocol |
| Entra ID | Illicit OAuth consent | Consent to application, mail scopes |
| Entra ID | App registration secret add | Add service principal credentials |
| Entra ID | Global admin added | Add member to role |
| Entra ID | MFA method registration by attacker | User registered security info |
| Entra ID | Conditional access policy change | Update conditional access policy |
| Entra ID | Federation backdoor (Golden SAML) | Set domain authentication, ADFS token signing theft |
| Entra ID | PRT theft and pass the PRT | Sign in from new device with PRT |
| M365 | Malicious inbox rule | New-InboxRule, Set-InboxRule |
| M365 | Mail forwarding to external | Set-Mailbox ForwardingSmtpAddress |
| M365 | Mailbox access by attacker | MailItemsAccessed |
| M365 | eDiscovery search abuse | New-ComplianceSearch, export |
| M365 | SharePoint mass download | FileDownloaded burst |
| M365 | Teams external phishing | Chat from external tenant |
| M365 | Mailbox delegation added | Add-MailboxPermission FullAccess |
| GCP | Service account key creation | CreateServiceAccountKey |
| GCP | IAM policy change | SetIamPolicy |
| GCP | Logging sink delete | DeleteSink |
| Kubernetes | Anonymous API access | Requests from system:anonymous |
| Kubernetes | Privileged pod creation | Pod with hostPID, hostPath / |
| Kubernetes | Exec into pod | pods/exec |
| Kubernetes | Secret read | get secrets burst |
| Kubernetes | Malicious image deploy | New deployment with miner image |
| Kubernetes | RBAC escalation | ClusterRoleBinding to cluster-admin |
| SaaS | GitHub token abuse | Clone of private repos, new deploy key |
| SaaS | Okta session hijack | Session reuse from new IP |

### Web Application Attacks

All of these produce WEB and PCAP samples. Capture HTTP in clear text or decrypt TLS with a key log file so the payloads show in the pcap.

| Attack | Key evidence |
| --- | --- |
| SQL injection (union, error, boolean, time based) | UNION SELECT, sleep, quotes in parameters |
| sqlmap automated SQLi | sqlmap user agent, huge request count |
| Blind SQLi data dump | Many requests with changing char positions |
| NoSQL injection | `$ne`, `$gt` operators in JSON |
| Reflected XSS | script tags in GET parameters |
| Stored XSS | Script in POST, later served to users |
| DOM XSS | Payload in URL fragment (BROW) |
| CSRF | Cross origin POST with no token |
| Command injection | Semicolons, pipes, backticks in parameters |
| Local file inclusion | ../../etc/passwd, php://filter |
| Remote file inclusion | Parameter with external URL |
| Path traversal | Encoded dot dot slash sequences |
| File upload to webshell | Upload of .php, .aspx, double extension |
| Webshell usage (China Chopper, Godzilla, Behinder) | Encrypted POST bodies to one file |
| SSRF | Requests to 169.254.169.254, localhost |
| XXE | DOCTYPE with external entity |
| Insecure deserialization (Java, PHP, .NET) | ysoserial gadget bytes, O: serialized objects |
| Server side template injection | {{7\*7}} style payloads |
| Log4Shell and JNDI injection | jndi ldap strings in headers |
| Spring4Shell | class.module.classLoader parameters |
| Shellshock | () { :; }; in headers |
| HTTP request smuggling | Conflicting Content-Length and Transfer-Encoding |
| Host header injection | Odd Host value in reset links |
| Open redirect abuse | Redirect parameter to evil site |
| IDOR | Sequential ID changes by one user |
| Broken auth and JWT tampering | alg none, changed claims |
| Session hijacking | Same session cookie from two IPs |
| Login brute force | POST burst to login page |
| Credential stuffing | Many users, one try each |
| Account takeover by password reset abuse | Reset flood, token reuse |
| API abuse and enumeration | Burst on API endpoints, 401 and 403 |
| GraphQL introspection and batching | \_\_schema queries |
| Mass assignment | Extra role fields in JSON |
| Clickjacking | Page framed by other site (BROW) |
| Web cache poisoning | Unkeyed headers with payload |
| WordPress plugin exploit | wp-content plugin path exploit |
| Directory listing and backup file hunting | Requests for .bak, .git, .env |
| Exposed .git and .env download | 200 on /.git/config |
| Web scraping bots | Fast sequential crawl |
| Layer 7 DDoS | Flood on heavy endpoints |

### Network and Infrastructure Attacks

All of these produce PCAP samples. Add device logs (router, switch, firewall syslog) where you can.

| Attack | Key evidence |
| --- | --- |
| ARP spoofing and MITM | Duplicate IP to MAC mappings, gratuitous ARP |
| DHCP spoofing (rogue DHCP) | Two DHCP servers answering |
| DHCP starvation | Flood of DISCOVER with random MACs |
| DNS spoofing and cache poisoning | Fake answers, mismatched TXID |
| DNS hijack by changed resolver | Clients using unknown DNS server |
| IPv6 RA spoofing and mitm6 | Rogue router advertisements, DHCPv6 replies |
| MAC flooding | CAM table overflow traffic |
| VLAN hopping (double tagging, DTP) | Double 802.1Q tags, DTP frames |
| STP manipulation | BPDU claiming root bridge |
| HSRP and VRRP hijack | Higher priority hello from attacker |
| BGP hijack | Route announce changes (device logs) |
| SSL stripping | HTTPS downgraded to HTTP |
| TLS downgrade and weak cipher | Old TLS versions negotiated |
| SMB relay | SMB auth forwarded to other host |
| SYN flood | Many half open connections |
| UDP flood | High UDP rate to one target |
| Ping of death and fragmentation attacks | Overlapping or huge fragments |
| Smurf and Fraggle | ICMP or UDP to broadcast |
| NTP and DNS amplification | Small queries, huge replies |
| Teardrop and LAND attacks | Bad fragment offsets, same src and dst |
| Session hijacking TCP | Injected packets with right sequence numbers |
| Telnet and FTP cleartext abuse | Plain text creds and commands |
| SNMP default community abuse | public or private strings, SET requests |
| Modbus and DNP3 attacks (ICS) | Write coil or register commands from new host |
| S7comm PLC stop | Stop CPU function codes |
| VoIP SIP flood and toll fraud | INVITE flood, odd call destinations |
| WiFi deauth attack | Burst of deauth frames |
| WiFi WPA handshake capture | EAPOL frames after deauth |
| WiFi evil twin | Same SSID from new BSSID |
| WiFi PMKID attack | EAPOL M1 requests to AP |
| WiFi KRACK | Replayed handshake messages |
| Bluetooth attacks | HCI logs with odd pairing |
| Port knocking and covert channels | Odd sequences before port opens |
| Firewall evasion with fragmentation | Tiny fragments hiding headers |

### Email Attacks

All of these produce MAIL samples (.eml, .msg). Add CLOUD message trace and BROW click history for the full picture.

| Attack | Key evidence |
| --- | --- |
| Credential phishing link | Fake login URL, lookalike domain |
| Malicious attachment (macro, ISO, LNK, HTML) | Attachment type and hash |
| Display name spoofing | CEO name with free mail address |
| Domain spoofing | SPF, DKIM, DMARC fail in headers |
| Lookalike domain (typosquat, homoglyph) | rn for m, Cyrillic letters |
| Reply chain hijack (thread hijacking) | Real thread, new sender IP, payload link |
| BEC invoice and payment change | Bank details change request |
| CEO fraud gift card scam | Urgent request, no links |
| Vendor email compromise | Real vendor domain, new reply to |
| QR code phishing | Image only body |
| Callback phishing | Phone number, no link |
| Open redirect in link | Trusted domain redirecting out |
| URL shortener and cloud hosted phish | bit.ly, SharePoint, Google Forms links |
| Encrypted or password protected attachment | Password in body |
| Calendar invite phishing | .ics with malicious link |
| Sextortion and extortion spam | Bitcoin address, threats |
| Malware spam campaign (Emotet, Qakbot style) | Many recipients, zipped payload |
| Header injection and reply to mismatch | Reply-To differs from From |
| Internal phishing from compromised account | Internal sender, mass recipients |
| Email bombing (subscription flood) | Thousands of signup confirmations |

### Malware Family Samples

For real malware, do not run it yourself. Use public captures and memory images from sources like Malware Traffic Analysis, MalwareBazaar and the Volatility sample sets, or simulate behavior with open source C2 frameworks.

| Malware type | Samples | Key evidence |
| --- | --- | --- |
| Infostealer (RedLine, Lumma, Vidar, Raccoon) | PCAP, MEM, BROW, MFT | Browser DB access, POST of zip to C2 |
| Banking trojan (Qakbot, Emotet, TrickBot, IcedID) | PCAP, MEM, REG, EVTX | Injection into explorer, scheduled task |
| Loader (GootLoader, Bumblebee, SocGholish, Latrodectus) | PCAP, EVTX, FILE | JS or MSI loader, follow on payload |
| RAT (AsyncRAT, njRAT, Remcos, QuasarRAT, DarkComet) | PCAP, MEM, REG | Run key, custom TCP C2 |
| Keylogger (Agent Tesla, Snake) | PCAP, MEM | SMTP or FTP exfil of keystrokes |
| Ransomware (LockBit, BlackCat, Conti, Akira, Ryuk) | MFT, EVTX, MEM, DISK | Encryption, note, shadow delete |
| Wiper (NotPetya, HermeticWiper, WhisperGate) | DISK, EVTX | MBR overwrite, driver load |
| Worm (WannaCry, Conficker) | PCAP, MEM | SMB scanning and exploit |
| Botnet (Mirai, Gafgyt) | PCAP, LNX | Telnet brute, DDoS commands |
| Cryptominer (XMRig) | PCAP, LNX, EVTX | Stratum traffic, high CPU |
| Rootkit (TDSS, ZeroAccess, Diamorphine) | MEM, DISK | Hooks, hidden processes |
| Bootkit (BlackLotus, TDL4) | DISK | Modified boot files |
| Post exploitation framework (Cobalt Strike, Sliver, Metasploit) | PCAP, MEM, EVTX | Beacon config, named pipes |
| Webshell families (China Chopper, ASPXSpy, P.A.S.) | WEB, MFT | Small script in web root |
| Fileless PowerShell malware | EVTX, MEM, REG | Payload stored in registry or WMI |
| Macro malware documents | FILE | VBA streams, OLE objects |
| PDF malware | FILE | JavaScript and OpenAction in PDF |
| Android malware (APK) | FILE, PCAP | Overlay permissions, SMS read |
| macOS stealers | FILE, LNX, PCAP | DMG, osascript prompts |
| Linux backdoors (BPFDoor, XorDDoS) | LNX, MEM, PCAP | Magic packet triggers, raw sockets |

### Full Scenario Packs

Each pack chains attacks from the sections above into one case with one answer key.

| Scenario | Attack chain | Samples |
| --- | --- | --- |
| Ransomware domain takeover | Phishing ISO, Cobalt Strike, Kerberoasting, PsExec, shadow delete, encryption | MAIL, PCAP, EVTX, MFT, MEM, DISK |
| Insider data theft | Normal login, share browsing, archive, USB copy, cloud upload | REG, EXEC, MFT, BROW, CLOUD |
| Web server compromise | Directory brute force, SQLi, webshell upload, privilege escalation, miner | WEB, PCAP, LNX, DISK, MEM |
| Cloud breach | Leaked key, IAM escalation, CloudTrail stop, S3 theft | CLOUD |
| M365 BEC | AiTM phishing, session replay, inbox rule, payment change email | MAIL, CLOUD, BROW |
| Active Directory attack lab | LLMNR poisoning, relay, DCSync, Golden Ticket | PCAP, EVTX, MEM |
| APT espionage | Spearphish, DLL side loading, WMI persistence, slow DNS exfil | PCAP, EVTX, REG, MEM, DISK |
| Supply chain | Trojanized update, beacon, lateral to servers | PCAP, EVTX, DISK |
| Anti forensics case | Timestomp, log clear, SDelete, `$UsnJrnl` delete | MFT, EVTX, DISK |
| Linux server intrusion | SSH brute force, sudo abuse, cron persistence, rootkit | LNX, PCAP, MEM, DISK |
| Kubernetes compromise | Exposed API, privileged pod, host escape, miner | CLOUD, LNX, PCAP |
| ICS intrusion | IT phishing, pivot to OT, Modbus writes | PCAP, EVTX |
| Wireless intrusion | Deauth, handshake capture, evil twin, internal scan | PCAP |

For every sample include: scenario story, ground truth timeline, ATT&CK IDs, SHA256 hashes, and tools and lab setup.

