# Index

- [Practical No. 1](#practical-no-1)
  - [A. Footprinting and reconnaissance](#a-footprinting-and-reconnaissance)
    - [i. Recon-ng (Kali Linux)](#i-recon-ng-kali-linux)
    - [ii. FOCA](#ii-foca-fingerprinting-organizations-with-collected-archives)
    - [iii. Windows Command Line Utilities](#iii-windows-command-line-utilities)
    - [iv. HTTrack (Website Copier)](#iv-httrack-website-copier)
    - [v. Metasploit (information gathering)](#v-metasploit-information-gathering)
    - [vi. WHOIS lookup (web and mobile)](#vi-whois-lookup-web-and-mobile)
    - [vii. eMailTracker Pro](#vii-emailtracker-pro)
    - [viii. Mobile tools (footprinting/recon)](#viii-mobile-tools-footprintingrecon)
  - [B. Network scanning](#b-network-scanning)
    - [i. Hping2 / Hping3](#i-hping2--hping3)
    - [ii. Advanced IP Scanner](#ii-advanced-ip-scanner)
    - [iii. Angry IP Scanner](#iii-angry-ip-scanner)
    - [iv. Masscan](#iv-masscan)
    - [v. NEET](#v-neet)
    - [vi. CurrPorts](#vi-currports)
    - [vii. Colasoft Packet Builder](#vii-colasoft-packet-builder)
    - [viii. The Dude (MikroTik)](#viii-the-dude-mikrotik)
- [Practical No. 2](#practical-no-2)
  - [a. Proxy Workbench](#a-proxy-workbench)
  - [b. Network discovery](#b-network-discovery)
  - [c. Censorship circumvention tools](#c-censorship-circumvention-tools)
  - [d. Mobile scanning tools](#d-mobile-scanning-tools)
- [Practical No. 3](#practical-no-3)
  - [a. Enumeration](#a-enumeration)
  - [b. Vulnerability analysis](#b-vulnerability-analysis)
- [Practical No. 4](#practical-no-4)
  - [a. Mobile network scanning with Nessus](#a-mobile-network-scanning-with-nessus)
  - [b. System hacking](#b-system-hacking)
    - [i. WinRTGen (rainbow tables)](#i-winrtgen-rainbow-tables)
    - [ii. PWDump](#ii-pwdump)
    - [iii. Ophcrack](#iii-ophcrack)
    - [iv. FlexiSPY](#iv-flexispy)
    - [v. NTFS alternate data streams (ADS)](#v-ntfs-alternate-data-streams-ads)
    - [vi. ADS Spy](#vi-ads-spy)
    - [vii. Snow (text steganography)](#vii-snow-text-steganography)
    - [viii. QuickStego (image steganography)](#viii-quickstego-image-steganography)
    - [ix. Clearing audit policies](#ix-clearing-audit-policies)
    - [x. Clearing logs](#x-clearing-logs)
- [Practical No. 5](#practical-no-5)
  - [a. Wireshark (network sniffing)](#a-wireshark-network-sniffing)
  - [b. SMAC (MAC spoofing)](#b-smac-mac-spoofing)
  - [c. Capsa Network Analyzer](#c-capsa-network-analyzer)
  - [d. Omnipeek](#d-omnipeek)
- [Practical No. 6](#practical-no-6)
  - [a. Social Engineering Toolkit (SET) on Kali](#a-social-engineering-toolkit-set-on-kali)
  - [b. DDoS demonstration tools](#b-ddos-demonstration-tools)
  - [c. Burp Suite](#c-burp-suite)
- [Practical No. 7](#practical-no-7)
  - [a. OWASP ZAP – web app scanning](#a-owasp-zap-zed-attack-proxy--web-app-scanning)
  - [b. DroidSheep (session hijacking on mobile)](#b-droidsheep-session-hijacking-on-mobile)
  - [c. Firewalls](#c-firewalls)
  - [d. HoneyBOT (malicious traffic capture)](#d-honeybot-malicious-traffic-capture)
  - [e. Web server protection tools](#e-web-server-protection-tools)
- [Practical No. 8](#practical-no-8)
  - [a. dotDefender (web application protection)](#a-dotdefender-web-application-protection)
  - [b. SQL injection demonstration tools](#b-sql-injection-demonstration-tools)
- [Practical No. 9](#practical-no-9)
  - [Aircrack-ng (wireless hacking and countermeasures)](#aircrack-ng-wireless-hacking-and-countermeasures)
- [Practical No. 10](#practical-no-10)
  - [Cryptography tools](#cryptography-tools)
    - [i. HashCalc](#i-hashcalc)
    - [ii. Advanced Encryption Package](#ii-advanced-encryption-package)
    - [iii. TrueCrypt](#iii-truecrypt)
    - [iv. CrypTool](#iv-cryptool)

---

# Practical No. 1

## A. Footprinting and reconnaissance

**Purpose:** Collect basic information about the target (IP, routing, business, address, DNS records) before exploitation.

### i. Recon-ng (Kali Linux)

Recon-ng is a web reconnaissance framework written in Python. It has independent modules and database support. Used for information gathering and network detection. Download from www.bitbucket.org; requires Kali Linux.

**Steps:**
1. Open Kali terminal and run `recon-ng`.
2. Type `show modules` to list all available modules.
3. Search for a module, e.g. type `search Netcraft`.
4. Load the module: `use recon/domain-hosts/Netcraft`, press Enter.
5. Set the target: `set source [domain]`, then type `Run` to execute.

### ii. FOCA (Fingerprinting Organizations with Collected Archives)

FOCA finds metadata and hidden information inside documents on web pages (e.g. author, creation date). Supports Office, PDF, Adobe InDesign, SVG, etc. Uses Google, Bing, and DuckDuckGo for search.

**Steps:**
1. Download from https://www.elevenpaths.com. Go to **Project > New Project**.
2. Enter Project Name, Domain/Website, directory to save results, Project Date. Click **Create**.
3. Select search engines, file extensions, and other options. Click **Search All**.
4. When search completes, select files, download them, and use **Extract Metadata** to get username, file creation date, and modification info.

### iii. Windows Command Line Utilities

Use a Windows PC with internet; assume a target like example.com.

**Ping**
- Open CMD. Run `Ping example.com`.
- **What you observe:** Target is live; IP address of example.com; Round Trip Time (RTT); TTL value; packet loss.
- To test fragmentation: `Ping example.com –f –l 1500`. If you see "Packet needs to be fragmented but DF set", reduce the size (e.g. 1400, 1300, 1200) until you get a reply. That shows the max unfragmented size.

**Tracert**
- Run `Tracert example.com`.
- **What you observe:** Hops from your PC to the destination; response time per hop; path. You can use this to map the network (first hop = gateway; trace different IPs to see which devices connect to which).

**Traceroute tools (optional):** Path Analyzer Pro, Visual Route, Troute, 3D Traceroute (give graphical view).

**NSLookup (DNS Zone Transfer)**

Nslookup queries the DNS server. DNS zone transfer (TCP port 53) copies DNS records from one server to another. If misconfigured, an attacker can get the full zone (all hostnames).

**Steps:**
1. Open CMD, type `nslookup`, Enter.
2. At the `>` prompt: `server <DNS Server Name or IP>`.
3. `set type=any` — retrieves all record types.
4. `ls -d <Domain>` — lists domain info if the server allows zone transfer.
5. If not allowed, you get "request failed".
6. On Linux you can use: `dig <domain.com> axfr`.

### iv. HTTrack (Website Copier)

HTTrack copies a website to your PC so you can browse it offline and study its structure.

**Steps:**
1. Download from http://www.httrack.com (Windows/Linux/Android).
2. Install and start a new project (e.g. name: Testing_Project).
3. Click **Set Options**, go to **Scan Rules** if needed.
4. Enter the web address (e.g. example.com), click **Next**.
5. When done, click **Browse Mirrored Website** and choose a browser.
6. **Observation:** The site is in a local folder; compare with the original URL to confirm it mirrors correctly.

### v. Metasploit (information gathering)

Metasploit on Kali is used for discovery and exploitation: scan ports/services, exploit vulnerabilities, collect evidence. In the lab we use a private network (e.g. 10.10.50.0/24) with Windows 7, Kali, Windows Server 2016.

**Steps:**
1. Open Kali, run Metasploit (e.g. `msfconsole`).
2. Use appropriate modules (e.g. SMB scan for OS).
3. Type `hosts` to see discovered hosts.
4. **Observation:** Check the **OS_Flavor** field — SMB scanning identifies the operating system for the scanned RHOST range.

### vi. WHOIS lookup (web and mobile)

WHOIS gives domain ownership, IP, netblock, DNS servers, ASN, domain status, history. Regional Internet Registries (RIR) maintain the database. Use https://www.whois.com or https://whois.domaintools.com; enter target URL. **SmartWhois** (www.tamos.com) is a desktop WHOIS tool.

**What the result includes:** Registrant info, organization, country, name servers, IP, ASN, domain status, WHOIS/IP/registrar/hosting history, and often email/postal address of registrar and admin.

### vii. eMailTracker Pro

Windows-based email tracker. Used to trace where an email came from using the message header.

**Steps:** Open the tool → **Trace Headers** or **Trace email address** → paste the full email message header → click OK. The trace result appears in **Trace Reports**.

### viii. Mobile tools (footprinting/recon)

Use from a mobile device: **DNS Tools**, **Whois**, **Ultra Tools Mobile**, **Network Scanner**, **Fing – Network Tool**, **Network Discovery Tool**, **Port Droid**. They help perform footprinting and reconnaissance from a phone.

---

## B. Network scanning

**Purpose:** Identify live hosts, open ports, and services on the network.

### i. Hping2 / Hping3

Command-line tool to build and send custom TCP/IP packets and read replies. Used to test firewall rules, do port scanning, path MTU discovery, traceroute, and OS fingerprinting. Supports TCP, UDP, ICMP.

**Example commands (Kali):**
- ACK packet: `hping3 –A 192.168.0.1`
- SYN scan on ports 1–600: `hping3 -8 1-600 –S 10.10.50.202`
- Packet with FIN, URG, PSH: `hping3 –F –P -U 10.10.50.202`

### ii. Advanced IP Scanner

Fast network scanner with a simple interface. Finds all computers on the LAN and scans their ports. Gives access to HTTP, HTTPS, FTP, and shared folders.

### iii. Angry IP Scanner

Open-source, cross-platform. Scans IP addresses and ports quickly. Used by admins and in enterprises.

### iv. Masscan

High-speed TCP port scanner; sends SYN packets asynchronously (similar idea to nmap but very fast). Example: `masscan -p22,80,445 192.168.1.0/24` to scan those ports on the subnet.

### v. NEET

Multi-threaded network penetration testing tool. Coordinates several open-source tools to gather network info and run tests. Sits between manual scanning and full automated vulnerability scanners.

### vi. CurrPorts

Shows all current TCP/IP connections (process name, protocol, local/remote port, IP). Used to detect and kill unwanted connections (e.g. from a trojan).

**Example use:** Run CurrPorts on Windows Server, then run the HTTP RAT from the previous lab. The new process and its connection appear in the list. Right-click the process → Properties for details. From another machine, connect via browser; back on the server, kill the connection. Verify by trying to connect again from the client.

### vii. Colasoft Packet Builder

Creates custom network packets (ARP, IP, TCP, UDP) for testing or attacks. Download from www.colasoft.com. Add packet → choose type → set fields → select adapter → send.

### viii. The Dude (MikroTik)

Network monitor: auto-scans subnets, draws a map, monitors devices and links, sends alerts. Supports SNMP, ICMP, DNS, TCP; link usage graphs; remote management. Can run as server with local client.

---

# Practical No. 2

## a. Proxy Workbench

A proxy server that shows data in real time (e.g. between email client and server, browser and web server, FTP). Useful for developers and security analysis. You can see the traffic and save it to a file. The connection diagram can be exported to HTML.

**Steps:** Run Proxy Workbench, configure it as the proxy for your client (browser/email). Perform some activity; observe the data in the tool and save to file as required.

## b. Network discovery

**SolarWinds Network Topology Mapper (NTM):** Discovers nodes and links, shows status on scalable maps with customizable icons.

**OpManager:** Network monitoring — fault and performance management for WAN, routers, switches, VoIP, servers.

**Network View:** Discovers and monitors multi-vendor IP networks using ICMP, DNS, NetBIOS, SNMP, LLDP, etc. Shows logical and physical topology; supports wireless (Cisco, Aruba, Fortinet).

**LANState Pro:** Topology mapping, host monitoring, service availability. Manage devices via graphic map; quick access to RDP and web UI.

## c. Censorship circumvention tools

**Alkasir:** Bypasses ISP restrictions so users can access information that may be blocked for political or regional reasons.

**Tails OS (The Amnesic Incognito Live System):** Live OS focused on privacy and anonymity. Used by journalists and activists; leaves no trace on the host machine when used from USB.

## d. Mobile scanning tools

Use from mobile: **Network Scanner**, **Fing – Network Tool**, **Network Discovery Tool**, **Port Droid**. Available on app stores; used for network scanning from a phone.

---

# Practical No. 3

## a. Enumeration

**Purpose:** Identify hosts, OS, shares, users, and services on the network.

- **Nmap:** Network scanner. OS detection: `nmap -O <ip address>`. Sends probes and uses responses to guess the OS.
- **NetBIOS enumeration:** NetBIOS = Network Basic Input/Output System. Used on Windows for file/printer sharing over the LAN. NetBIOS names identify machines; enumeration tools list shares and users.
- **SuperScan:** Multi-purpose tool; fast TCP port and connection scanning.
- **Hyena:** GUI tool for NetBIOS enumeration — shows shares, user login info, and related data.
- **SoftPerfect Network Scanner:** Pings hosts, scans ports, finds shared folders; can use WMI, SNMP, HTTP, SSH, PowerShell to get device info.
- **OpUtils:** Manages IP addresses and switch ports; monitoring and troubleshooting.
- **SolarWinds Engineer’s Toolset:** Collection of diagnostic, performance, and bandwidth tools for network engineers.
- **Wireshark:** Packet analyzer for troubleshooting, protocol analysis, and education. Used to inspect traffic during enumeration.

## b. Vulnerability analysis

**Nessus (Tenable):** Vulnerability scanner. Finds weaknesses that attackers could exploit. Uses CVE; used in vulnerability assessments and penetration tests. Can be cloud (Tenable.io) or on-prem.

**OpenVAS:** Open-source, full-featured vulnerability scanner. Supports unauthenticated and authenticated scans, many protocols, large-scale tuning, and custom tests. Gets vulnerability tests from a feed (Greenbone) with daily updates.

---

# Practical No. 4

## a. Mobile network scanning with Nessus

Normal network scanning is not ideal for mobile devices (often on 3G/4G or in sleep). Nessus can use **MDM (Mobile Device Management)** and **Active Directory / Microsoft Exchange** to get information about registered phones.

**Requirements:** Nessus must reach the Mobile Device Management servers; give Nessus admin (e.g. domain admin) credentials for Active Directory. Configure authentication for the management server and mobile plugins. You do not need to define a list of hosts — Nessus talks to the management servers. For ActiveSync/Exchange, Nessus gets data for phones updated in the last 365 days.

## b. System hacking

### i. WinRTGen (rainbow tables)

Rainbow tables precompute hashes to speed up password cracking. WinRTGen generates these tables.

**Steps:**
1. Click **Add Table**. In **Rainbow Table Properties**, set **Hash** (e.g. MD5, SHA1).
2. Click **Benchmarks** to see estimated time, hash speed, and table size.
3. Click **OK**, then start the table generation. When finished, tables are saved in the WinRTGen directory.

### ii. PWDump

Windows stores user accounts and hashed passwords in the **SAM (Security Account Manager)**. SAM is in the registry at HKEY_LOCAL_MACHINE\SAM and on disk at C:\Windows\System32\config. PWDump dumps these hashes.

**Steps:** Run `PwDump7.exe` (after obtaining it). It outputs the hashes. To save SAM and SYSTEM for offline analysis: `reg save hklm\sam c:\sam` and `reg save hklm\system c:\system`.

### iii. Ophcrack

Free Windows password recovery using rainbow tables. Use when the PC is locked and you don’t know the password.

**Steps:**
1. On another PC with internet: download Ophcrack Live CD, burn to USB or CD.
2. Boot the locked PC from this media. Ophcrack loads (it has its own OS).
3. Choose default (automatic) mode. It finds the partition with the SAM file.
4. When done, a table shows user accounts and recovered passwords in the **NT Pwd** column.
5. Note the password, remove the media, restart, and log in with the recovered password.

### iv. FlexiSPY

Phone monitoring app (e.g. Android). Can record calls, capture SMS/WhatsApp, keystrokes, emails, and track the device. Can start recording remotely. Used here to demonstrate mobile monitoring concepts.

### v. NTFS alternate data streams (ADS)

NTFS has a main data stream and can have **alternate data streams (ADS)** attached to a file. ADS can hide data (or code) without changing the file’s visible size or content. Attackers sometimes use ADS to hide malware.

**Create and view ADS:**
- Create file: `echo "this is file no 1" > file_1.txt`
- Write to stream: `echo "hidden text" > file_1.txt:secret.txt`
- Open stream in Notepad: `notepad file_1.txt:secret.txt` (Notepad supports ADS). **Do not store sensitive data in ADS.**

**Hide executable in ADS:**
- `type Trojan.exe > note.txt:Trojan.exe` (copies exe into stream of note.txt)
- `mklink game.exe note.txt:Trojan.exe` (creates a link to run it)
- Running `game.exe` runs the hidden trojan. Original file size of note.txt does not change.

### vi. ADS Spy

Tool to find files that have alternate data streams. Options: Quick Scan, Full Scan, or Scan Specific Folder. Run scan → it lists files with ADS (e.g. Testfile.txt:hidden.txt). You can remove ADS from files if needed.

### vii. Snow (text steganography)

Hides text inside a text file using whitespace. The file looks normal.

**Hide:** `Snow –C –m "text to hide" –p "password" <sourcefile> <destfile>`. The destination file looks like the source but contains the hidden message.

**Recover:** `Snow –C –p "password" destfile`. The hidden text is displayed.

### viii. QuickStego (image steganography)

Hides text inside an image. The image is the **cover**; the result is the **stego object**.

**Steps:** Open QuickStego → load image → enter text or load a text file → **Hide Text** → save image. To recover: open image → **Get Text**. You can compare the original and stego images (they look the same; only the hidden data differs).

### ix. Clearing audit policies

Windows **auditpol** controls what is logged (e.g. logon events, system events).

- View options: `auditpol /?`
- Enable auditing for System and Account logon: `auditpol /set /category:"System","Account logon" /success:enable /failure:enable`
- Check: `auditpol /get /category:"Account logon","System"`
- Clear all audit policy: `auditpol /clear` (confirm with Y). After clearing, check again to confirm policies are reset.

### x. Clearing logs

On Kali (or Linux): go to `/var/log`, open the **log** folder, select the log file you want to clear, and delete or truncate it. Demonstrates how an attacker might remove traces of activity.

---

# Practical No. 5

## a. Wireshark (network sniffing)

Wireshark captures and displays network traffic (frames/packets). Useful to see what is sent and received on the wire.

**Steps:**
1. Start Wireshark. Under **Capture**, open **Interface List** (or click **Interfaces**).
2. Select the adapter you use for internet (wired or wireless), click **Start**.
3. Generate traffic (e.g. browse a few websites). Packets appear; they are color-coded by type.
4. Use the **Filter** bar (e.g. type `http`) and click **Apply** to see only HTTP.
5. Click an HTTP request packet. In the middle pane, expand **Hypertext Transfer Protocol** to see GET, Host, User-Agent, cookies, etc.
6. Click the corresponding response (e.g. HTTP/1.1 200 OK) to see Cache-Control, Content-Type, Server, etc.
7. Stop the capture when done.

## b. SMAC (MAC spoofing)

SMAC changes the MAC address of a network adapter. Useful for privacy or testing. Has a GUI; can randomize or choose a manufacturer.

**Steps:** Install SMAC (under KLC in Start menu). Open SMAC → select the adapter (uncheck “show only active” to see all). Click **Random** (or pick manufacturer) to get a new MAC. In **Options**, enable **Automatically Restart Adapter**. Click **Update MAC Address**. The new MAC is applied.

## c. Capsa Network Analyzer

Network analyzer for capture and analysis. **Steps:** Double-click desktop icon → on Start Page select NIC(s) in Capture panel → choose Network Profile → choose **Full Analysis** → click **Run**. Flow: Select NIC → Profile → Analysis → Run.

## d. Omnipeek

Protocol analyzer for wired and wireless networks. Supports high-speed and 802.11; decoding of many protocols; visualization; WiFi adapter for capture; LiveCapture for remote capture; voice and video analysis.

---

# Practical No. 6

## a. Social Engineering Toolkit (SET) on Kali

SET is used to clone a website and harvest credentials. When the victim visits the clone and enters username/password, SET captures them and redirects the victim to the real site.

**Steps:**
1. Open Kali → **Applications** → **Social Engineering Tools** → **Social Engineering Toolkit**.
2. Enter **Y** to continue.
3. Type **1** (Social Engineering Attacks) → **2** (Website attack vector) → **3** (Credentials harvester) → **2** (Site Cloner).
4. Enter the IP of your Kali machine (e.g. 10.10.50.200) and the target URL to clone.
5. Send the victim the clone link (e.g. http://10.10.50.200). In real attacks this would be hidden behind a fake URL.
6. When the victim logs in (e.g. admin / Admin@123), the credentials appear in the Kali terminal. The victim is then redirected to the real site so they may not notice.

## b. DDoS demonstration tools

**HOIC (High Orbit Ion Cannon):** Sends many HTTP GET/POST requests. Can open many sessions; used for stress/DoS testing.

**LOIC (Low Orbit Ion Cannon):** Simple DoS tool. Enter target URL, click **Lock On**, choose TCP, UDP, or HTTP. Click the big button (“IMMA CHARGIN MAH LAZER”) to start. Adjust threads/speed. When request count stops increasing, you may need to restart or change IP.

**HULK (HTTP Unbearable Load King):** Sends many HTTP GET requests with random elements. Used to test if a server can handle load.

**Metasploit SYN flood:** Get target IP (e.g. from domain with nslookup/ping). In Kali: `msfconsole` → `use auxiliary/dos/tcp/synflood` → `set RHOST <target IP>` → `set RPORT 80` → `exploit`. Use Wireshark to see SYN packets. **Use only in authorized lab.**

## c. Burp Suite

Burp Suite acts as a proxy between the browser and the web application. You can inspect and modify HTTP requests and responses (e.g. for penetration testing).

**Steps:** Configure the browser to use Burp as proxy. Turn on “Intercept” in Burp. Browse to the target app; requests appear in Burp. You can forward, drop, or edit them and observe the effect on the application.

---

# Practical No. 7

## a. OWASP ZAP (Zed Attack Proxy) – web app scanning

ZAP is a free, open-source web application security scanner. It crawls the site and then scans for vulnerabilities.

**Steps:**
1. Start ZAP. Open the **Quick Start** tab.
2. Click **Automated Scan**.
3. Enter the full URL of the web application.
4. Click **Attack**. ZAP crawls with its spider and passively scans each page, then actively attacks discovered pages and parameters. There are two spiders (traditional HTML-based; for AJAX/JS sites you may use both).

## b. DroidSheep (session hijacking on mobile)

Android tool for session hijacking (sidejacking). On the same Wi-Fi, it captures HTTP traffic and extracts session IDs so you can reuse someone else’s session. Supports open, WEP, and WPA/WPA2 (PSK). Uses libpcap and arpspoof. **Use only in authorized lab.**

## c. Firewalls

**ZoneAlarm:** Open from desktop icon or Start → Check Point → ZoneAlarm. Main screen shows whether the computer is SECURE or AT RISK. Panels: **Antivirus & Firewall** (antivirus, firewall, application control, threat emulation), **Web & Privacy** (e.g. anti-keylogger), **Mobility & Data** (e.g. identity protection).

**Comodo Firewall:** Open **Tasks** → **Firewall Tasks**. Benefits: monitors inbound/outbound traffic, hides ports, blocks malware from sending data. Configure per-application access, stealth mode, view connections, or block all. Advanced options under **Settings** → **Firewall**.

## d. HoneyBOT (malicious traffic capture)

Scripts that capture traffic and send it to PacketTotal.com for analysis. Scripts: `capture-and-analyze.py`, `upload-and-analyze.py`, `trigger-and-analyze.py`. Options include `--seconds`, `--interface`, `--analyze`, `--list-interfaces`, `--list-pcaps`, `--export-pcaps`. Used to capture and analyze suspicious or malicious traffic.

## e. Web server protection tools

**ID Server:** Enter target URL or IP → click Query. It extracts and shows domain name, open ports, server type, and related info.

**Microsoft Baseline Security Analyzer (MBSA):** Scans for missing security updates and common misconfigurations. Can scan local machine, remote machine, or a range. Results: Security Update Scan Results (missing updates), Administrative Vulnerabilities (passwords, firewall, accounts, etc.), and optionally IIS, SQL Server, desktop applications.

**Syhunt Hybrid:** Supports dynamic, code, and log scanning. Go to Dynamic Scanning → enter URL or IP → run scan. Click each finding to see description and recommended solution.

---

# Practical No. 8

## a. dotDefender (web application protection)

dotDefender protects web applications (Apache and IIS) from common attacks. Central management for multiple servers. Modify the **Default Security Profile** or create/edit **Website Security Profiles** to set the level of protection.

## b. SQL injection demonstration tools

**Tyrant SQL:** GUI front-end for Sqlmap; cross-platform. Used to find and exploit SQL injection.

**Havij:** Automated SQL injection tool. Helps find and exploit SQL injection vulnerabilities in web pages (input fields, etc.).

**BBQSQL:** Blind SQL injection tool written in Python. Semi-automatic; good when injection is “blind” (no direct output). Database-agnostic; has a simple UI; uses gevent for speed. **Use only on authorized targets.**

---

# Practical No. 9

## Aircrack-ng (wireless hacking and countermeasures)

**Purpose:** Capture wireless (802.11) traffic and crack WPA/WPA2 password using a wordlist (e.g. generated with Cupp).

**Steps:**
1. Capture WLAN packets (e.g. in Wireshark with a suitable filter), save as a .cap file (e.g. WPA.cap).
2. On Kali: `cd Desktop`. Download Cupp: `git clone https://github.com/chetan31295/cupp.git`, then `cd cupp`.
3. Generate wordlist: run `./cupp.py` or `./cupp.py -i` for interactive. Enter target info (name, keywords, special chars, numbers); optionally enable leet mode. A text file is created (e.g. Albert.txt) with possible passwords.
4. Crack: `aircrack-ng –a 2 –b <BSSID of router> -w /root/Desktop/cupp/Albert.txt /root/Desktop/WPA.cap`. Option `-a 2` is for WPA/WPA2. The result either shows the key or reports that the dictionary did not contain it (then try a larger/different wordlist).

---

# Practical No. 10

## Cryptography tools

### i. HashCalc

Computes hashes (e.g. MD5, SHA) for files or text. **Steps:** Open HashCalc. For a file: set Data Format to **File**, select file, choose algorithm (e.g. MD5), click **Calculate**. For text: set Data Format to **Text String**, type the string (e.g. "IPSpecialist…"), click **Calculate**. **Observation:** A tiny change (e.g. one letter from upper to lower case) produces a completely different hash (avalanche effect).

### ii. Advanced Encryption Package

Encrypts/decrypts files with a password and chosen algorithm. **Steps:** Install (e.g. 2014/2017 for Windows 7/10). Select file → set password → choose algorithm → **Encrypt**. Copy the encrypted file to another PC; there open the package → **Decrypt** → enter password. File is restored.

### iii. TrueCrypt

Creates an encrypted container (volume) on disk. Data inside is protected by a password. **Steps:** Start TrueCrypt → create new volume → Next (standard volume) → Next → **Select File** and choose location and name (e.g. container.avi; avoid synced folders like Dropbox). Next → choose encryption/hash if needed → set volume size → set a strong password (24+ characters, mixed; max 64). Next → choose filesystem (NTFS or FAT). Move the mouse to add entropy → **Format**. The container is ready; mount it in TrueCrypt with the password to use it like a drive.

### iv. CrypTool

Free e-learning software for cryptography. Use it to try different encryption and decryption algorithms (e.g. AES, RSA, classical ciphers) and see how they work.
