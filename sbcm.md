# Practical No. 1

## A. Footprinting and reconnaissance

Collect basic target info (IP, routing, business, DNS) to support exploitation.

### i. Recon-ng (Kali Linux)
Web reconnaissance framework (Python, modules, DB). Download: www.bitbucket.org. Needs Kali.
- Run `recon-ng` in terminal.
- `show modules` — list modules. Search e.g. Netcraft.
- `use recon/domain-hosts/Netcraft` then `set source [domain]`, type `Run`.

### ii. FOCA
Fingerprinting Organizations with Collected Archives. Finds metadata/hidden info in docs on web (Office, PDF, SVG, etc.). Uses Google, Bing, DuckDuckGo.
- Download from https://www.elevenpaths.com → Project > New Project.
- Enter project name, domain, directory, date → Create. Set search engines/extensions → Search All.
- Select files, download, extract metadata (username, dates).

### iii. Windows CLI (example.com as target)
- **Ping:** `Ping example.com` — see live host, IP, RTT, TTL, packet loss. `Ping example.com –f –l 1500` to test fragmentation (reduce size until reply; ~1200 bytes may work).
- **Tracert:** `Tracert example.com` — hops, latency. Use result to map network (gateway = first hop; trace different IPs to see connected devices).
- **Traceroute tools:** Path Analyzer Pro, Visual Route, Troute, 3D Traceroute.
- **NSLookup (DNS zone transfer):** CMD → `nslookup` → `server <DNS>` → `set type=any` → `ls -d <Domain>`. Linux: `dig <domain.com> axfr`.

### iv. HTTrack
Download from http://www.httrack.com. New project → set options/scan rules → enter URL → Next → Browse Mirrored Website. Compare local copy with original site.

### v. Metasploit (info gathering)
Framework on Kali for discovery/exploitation. Lab: private net 10.10.50.0/24. Run `msfconsole` → use modules → `hosts` to see OS_Flavor from SMB scan.

### vi. WHOIS (web/mobile)
Domain ownership, IP, DNS, ASN, history. Use https://www.whois.com or https://whois.domaintools.com. **SmartWhois:** www.tamos.com.

### vii. eMailTracker Pro
Windows email tracker. Trace Headers / Trace email address → paste message header → see trace report.

### viii. Mobile tools
Network Scanner, Fing, Network Discovery Tool, Port Droid — for scanning from mobile.

## B. Network scanning

### i. Hping2/Hping3
CLI packet assembler/analyzer (TCP, UDP, ICMP). Test firewall, port scan, MTU, OS fingerprint.
- ACK: `hping3 –A 192.168.0.1`
- SYN scan: `hping3 -8 1-600 –S 10.10.50.202`
- FIN/URG/PSH: `hping3 –F –P -U 10.10.50.202`

### ii. Advanced IP Scanner
Fast scanner: find hosts, scan ports, access HTTP/FTP/shares.

### iii. Angry IP Scanner
Open-source, cross-platform; scans IPs and ports.

### iv. Masscan
Async SYN scanner. Example: `masscan -p22,80,445 192.168.1.0/24`

### v. NEET
Multi-threaded network pentest tool; coordinates scans and exploitation.

### vi. CurrPorts
View/kill TCP/IP connections (e.g. detect RAT — run CurrPorts, run trojan, see new process, kill connection).

### vii. Colasoft Packet Builder
Build custom packets (ARP, IP, TCP, UDP). www.colasoft.com. Add packet → set type → customize → send.

### viii. The Dude (MikroTik)
Auto-scan subnets, draw map, monitor devices, alerts. SNMP/ICMP/DNS/TCP monitoring, link graphs.

---

# Practical No. 2

## a. Proxy Workbench
Proxy that shows data in real time (email, browser, FTP). Animated connection diagram; export to HTML. Use to see traffic and save to file.

## b. Network discovery
- **SolarWinds NTM** — nodes and links, status, scalable maps.
- **OpManager** — fault/performance management (WAN, router, switch, VoIP, servers).
- **Network View** — discovery/monitoring of multi-vendor IP nets (ICMP, DNS, NetBIOS, SNMP, LLDP, etc.); logical/physical views.
- **LANState Pro** — topology map, host monitoring, RDP/web UI access.

## c. Censorship circumvention
- **Alkasir** — bypass ISP restrictions for regional info.
- **Tails OS** — The Amnesic Incognito Live System; privacy-focused OS for anonymous use.

## d. Mobile scanning
Use Network Scanner, Fing, Network Discovery Tool, Port Droid from app stores.

---

# Practical No. 3

## a. Enumeration
- **Nmap:** OS detection — `nmap -O <ip>`
- **NetBIOS:** LAN file/printer sharing; names identify Windows devices.
- **SuperScan:** Fast TCP/connection checks.
- **Hyena:** GUI NetBIOS — shares, user info.
- **SoftPerfect:** Ping, ports, shares via WMI, SNMP, HTTP, SSH, PowerShell.
- **OpUtils:** IP and switch port management, monitoring.
- **SolarWinds Engineer’s Toolset:** Diagnostics, performance, bandwidth.
- **Wireshark:** Packet analyzer (network troubleshooting, protocol analysis).

## b. Vulnerability analysis
- **Nessus (Tenable):** Finds vulnerabilities; CVE-based; used in assessments and pentests.
- **OpenVAS:** Full-featured scanner (un/auth tests, many protocols, tuning, scripted tests). Greenbone; daily feed updates.

---

# Practical No. 4

## a. Mobile scanning with Nessus
Nessus Mobile plugins use MDM and Active Directory/Exchange. Ensure scanner can reach MDM; give Nessus admin credentials to AD. Configure auth for management server and mobile plugins; no need to specify hosts. ActiveSync: data from phones updated in last 365 days.

## b. System hacking

### i. WinRTGen (rainbow tables)
Add Table → Rainbow Table Properties → set Hash (MD5, SHA1, etc.) → Benchmarks → OK → start generation. Tables saved in WinRTGen directory.

### ii. PWDump
SAM has hashed passwords (Registry: HKEY_LOCAL_MACHINE\SAM; file: C:\Windows\System32\config). Run `PwDump7.exe` to dump hashes. Save SAM/system: `reg save hklm\sam c:\sam` and `reg save hklm\system c:\system`

### iii. Ophcrack
Free Windows password recovery. On another PC: download Ophcrack Live CD, burn to USB/CD. Boot locked PC from media → automatic mode → SAM detected → NT Pwd column shows recovered password. Restart and log in.

### iv. FlexiSPY
Phone monitoring: calls, SMS, WhatsApp, keystrokes, email, tracking, remote recorder.

### v. NTFS streams (ADS)
NTFS has main stream + alternate data streams (ADS). ADS can hide data/code without changing file size. Create: `echo "hidden" > file.txt:secret.txt`. View: `notepad file.txt:secret.txt`. Hide trojan: `type Trojan.exe > note.txt:Trojan.exe` then `mklink game.exe note.txt:Trojan.exe`; run `game.exe`. Do not store sensitive data in ADS.

### vi. ADS Spy
Scans for ADS (Quick/Full/Specific folder). Use to find/remove files like Testfile.txt:hidden.txt.

### vii. Snow (steganography in text)
`Snow –C –m "text to hide" –p "password" <source> <dest>`. Dest file looks normal; recover with `Snow –C –p "password" destfile`.

### viii. QuickStego
Image steganography: open image (cover) → enter text or file → Hide Text → save (stego object). Get Text to recover. Compare images to see difference.

### ix. Clearing audit policies
`auditpol /?` for options. Enable: `auditpol /set /category:"System","Account logon" /success:enable /failure:enable`. Clear: `auditpol /clear` (confirm Y). Check: `auditpol /get /category:"Account logon","System"`

### x. Clearing logs
Kali: open /var → logs → select log file → delete as needed.

---

# Practical No. 5

## a. Wireshark (sniffing)
Start Wireshark → Interfaces → select adapter → Start. Browse web; packets appear (color-coded). Use Filter (e.g. `http`) to see only HTTP. Inspect GET request (Host, User-Agent, cookie) and response (Cache-Control, Content-Type, Server). Stop capture when done.

## b. SMAC (MAC spoofing)
Install SMAC (KLC folder). Select NIC → Random (or pick manufacturer) → Options → check Automatically Restart Adapter → Update MAC Address.

## c. Capsa Network Analyzer
Desktop icon → select NICs → Network Profile → Full Analysis → Run. Flow: NIC → Profile → Analysis → Run.

## d. Omnipeek
Protocol analyzer for wired/wireless and VoIP; visualization, WiFi adapter, remote capture (LiveCapture), voice/video analysis.

---

# Practical No. 6

## a. Social Engineering Toolkit (Kali)
Clone site and harvest credentials. Applications → Social Engineering Tools → SET → 1 (Social Engineering Attacks) → 2 (Website attack) → 3 (Credentials harvester) → 2 (Site Cloner). Enter Kali IP (e.g. 10.10.50.200) and target URL. Send victim clone link; when they log in (e.g. admin / Admin@123), credentials appear in terminal. Victim is redirected to real site.

## b. DDoS tools
- **HOIC:** Floods with HTTP GET/POST; up to 256 sessions.
- **LOIC:** Enter URL, Lock On, choose TCP/UDP/HTTP, click “IMMA CHARGIN MAH LAZER.” Adjust threads/speed.
- **HULK:** HTTP Unbearable Load King — random GETs; for testing server resilience.
- **Metasploit:** `msfconsole` → `use auxiliary/dos/tcp/synflood` → `set RHOST <target>` → `set RPORT 80` → `exploit`. Use Wireshark to see packets.

## c. Burp Suite
Intercept HTTP between browser and app. Configure proxy; traffic goes through Burp so you can inspect and modify requests/responses.

---

# Practical No. 7

## a. OWASP ZAP (web app scanning)
Quick Start → Automated Scan → enter URL → Attack. ZAP crawls with spider, passively scans, then actively attacks pages and parameters. Two spiders available (HTML-based; JS-heavy sites may need both).

## b. DroidSheep (mobile session hijacking)
Android; listens on Wi-Fi, extracts session IDs from HTTP. Supports open, WEP, WPA/WPA2 (PSK). Uses libpcap and arpspoof.

## c. Firewalls
- **ZoneAlarm:** Desktop icon or Start → Check Point → ZoneAlarm. Status: SECURE/AT RISK. Panels: Antivirus & Firewall, Web & Privacy, Mobility & Data.
- **Comodo:** Tasks → Firewall Tasks. Monitor traffic, hide ports, block malware; per-app rules, stealth, block all. Advanced: Settings → Firewall.

## d. HoneyBOT
Scripts for capture and analysis with PacketTotal.com: `capture-and-analyze.py`, `upload-and-analyze.py`, `trigger-and-analyze.py`. Options: `--seconds`, `--interface`, `--analyze`, `--list-interfaces`, `--list-pcaps`, `--export-pcaps`.

## e. Web server protection
- **ID Server:** Enter URL/IP → Query → copy result (domain, ports, server type).
- **MBSA:** Microsoft Baseline Security Analyzer — missing updates, misconfigurations; scan local/remote/range. Results: security updates, admin vulnerabilities, IIS, SQL, desktop apps.
- **Syhunt Hybrid:** Dynamic (and code/log) scanning. Enter URL → review findings and solutions.

---

# Practical No. 8

## a. dotDefender
Web app protection for Apache/IIS; central management. Modify Default or Website Security Profiles as needed.

## b. SQL injection tools
- **Tyrant SQL:** GUI for Sqlmap (cross-platform).
- **Havij:** Automated find/exploit of SQL injection on pages.
- **BBQSQL:** Blind SQL injection (Python), semi-automatic, DB-agnostic, fast (gevent).

---

# Practical No. 9

## Aircrack-ng (wireless)
1. Capture WLAN packets (e.g. Wireshark filter), save as .cap.
2. Kali: `cd Desktop` → `git clone https://github.com/chetan31295/cupp.git` → `cd cupp`.
3. Run `./cupp.py` or `./cupp.py -i` (interactive). Enter target info, keywords, special chars, numbers, leet. Output: wordlist file (e.g. Albert.txt).
4. Crack: `aircrack-ng –a 2 –b <BSSID> -w /root/Desktop/cupp/Albert.txt /root/Desktop/WPA.cap`. Result shows key or that dictionary failed.

---

# Practical No. 10

## Cryptography tools

### i. HashCalc
Open HashCalc. File: select file → algorithm → Calculate. Text: Data format “Text String”, enter text → Calculate. Small change (e.g. case) changes hash (e.g. IPSpecialist… vs IPspecialist…).

### ii. Advanced Encryption Package
Install (e.g. 2014/2017 for Win7/Win10). Select file → set password → choose algorithm → Encrypt. On other PC: open package → Decrypt → enter password.

### iii. TrueCrypt
Create encrypted container: Next → Select File → choose path and name → set size → strong password (24+ chars, mixed; max 64). Choose volume format (NTFS/FAT) → move mouse for entropy → Format. Container created.

### iv. CrypTool
Free crypto e-learning; try different encryption/decryption algorithms.
