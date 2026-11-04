# CEH v13 — Master Exam Review

**One consolidated final-review sheet — all 20 modules · Exam 312-50**

> Condensed from a full set of Version-3 module summaries. Built for the night before: enough to recognize the right answer in a scenario, not to re-teach the topic.
> *Mnemonics in the style of the Mr-Infect CEH v13 notes.*

---

## How to use this sheet

- **Part A — Quick Reference:** the facts most people miss on fine detail — ports, tools, the "don't get confused" pairs, frameworks and numbers. Read this first and last.
- **Part B — Module-by-Module:** one section per module — what to remember, the exam traps, and a mnemonic.

**How CEH questions work:** most questions are scenarios. The exam describes behavior and asks you to name the technique, tool, phase or port. Read for the keyword (hash, ticket, broadcast, half-open, TTL, stateless, no-click) and match it to the exact term.

---

## Part A — Quick Reference

### Important ports

| Port | Service | Why it matters for CEH |
|---|---|---|
| 20 / 21 | FTP | File transfer (plain text) |
| 22 | SSH / SFTP / SCP | Secure remote login (encrypted) |
| 23 | Telnet | Insecure remote login (plain text) |
| 25 | SMTP | Send email; SMTP enumeration (VRFY/EXPN) |
| 53 | DNS | Name resolution; zone transfer; amplification |
| 67 / 68 | DHCP | IP assignment; starvation / rogue DHCP |
| 69 | TFTP | Trivial FTP (plain text) |
| 80 | HTTP | Web (plain text); C2 channel |
| 110 | POP3 | Receive email (plain text) |
| 123 | NTP | Time; NTP enumeration; DDoS amplification |
| 137-139 | NetBIOS | Windows names / shares enumeration |
| 143 | IMAP | Email sync (plain text) |
| 161 / 162 | SNMP | Device management; public/private strings |
| 389 | LDAP | Directory / Active Directory enumeration |
| 443 | HTTPS | Secure web; encrypted C2 |
| 445 | SMB | Windows shares; EternalBlue (WannaCry) |
| 500 | IKE / IPsec | VPN key exchange |
| 1433 | MS-SQL | Microsoft SQL Server |
| 1883 | MQTT | IoT publish / subscribe broker |
| 1900 | SSDP | UPnP; DDoS amplification |
| 2049 | NFS | Unix / Linux file shares |
| 3306 | MySQL | Database |
| 3389 | RDP | Remote Desktop |
| 5683 | CoAP | IoT (UDP) |
| 6667 | IRC | Classic botnet C2 |


### Key tools by category

| Category | Tools |
|---|---|
| Recon / OSINT | Nmap, Shodan, Maltego, theHarvester, Recon-ng, WHOIS, Google Dorks |
| Scanning | Nmap, Zenmap, Masscan, Hping3, Angry IP Scanner |
| Enumeration | enum4linux, snmpwalk / snmp-check, ldapsearch, nbtstat, dnsenum, smtp-user-enum |
| Vuln assessment | Nessus, OpenVAS, Qualys, Nexpose, Nikto (web server) |
| Password cracking | Hashcat / John (offline), Hydra / Medusa (online), Ophcrack (rainbow), Mimikatz, Responder |
| Sniffing / MITM | Wireshark, tcpdump, Ettercap, Cain & Abel, macof, arpspoof, bettercap, SSLstrip |
| Web app / SQLi | Burp Suite, OWASP ZAP, SQLmap, Nikto, Dirb / Gobuster, BeEF, Havij |
| Wireless | aircrack-ng suite, Kismet, Reaver, Wash, Wifite |
| Mobile | Drozer, APKTool, JADX, MobSF, Frida, ADB, Cydia |
| IoT / OT | Shodan, Censys, RouterSploit, Metasploit |
| Cloud | Pacu, Prowler, ScoutSuite, CloudSploit |
| Exploitation | Metasploit (Exploit / Payload / Auxiliary / Encoder / NOP) |
| Malware analysis | IDA / Ghidra, PEiD / DIE, Strings (static); Cuckoo, Process Monitor (dynamic); VirusTotal |


### Don't get confused — the classic exam pairs

| Term | Means | Compare / remember |
|---|---|---|
| SYN scan (-sS) | half-open — sends RST, doesn't complete; stealthy | vs TCP connect (-sT) = completes handshake, logged |
| Idle / zombie (-sI) | source IP fully spoofed via a zombie | the only scan where your IP never appears in logs |
| Rainbow table | precomputed hash→password lookup (offline) | vs Pass-the-Hash = reuse the hash to log in, no cracking |
| Smurf | ICMP echo + broadcast + spoofed victim IP | vs Fraggle = same but UDP; vs Ping of Death = one oversized packet |
| SYN flood | protocol attack, half-open connections | NOT volumetric bandwidth; fix = SYN cookies |
| Session fixation | attacker sets the ID BEFORE login | fix = regenerate session ID AFTER login |
| Insertion attack | IDS accepts a packet the host rejects | vs Evasion = IDS rejects what the host accepts |
| Spoofing | start a NEW session with stolen creds | vs Hijacking = take over an EXISTING active session |
| Polymorphic | changes appearance (encrypts), logic same | vs Metamorphic = rewrites its whole code |
| Dropper | already carries the malware inside | vs Downloader = fetches malware from the internet |
| Virus | needs user action to spread | vs Worm = self-spreads, no user action |
| Tailgating | follow in WITHOUT consent | vs Piggybacking = WITH consent |
| Quid pro quo | offer a service in exchange for info | vs Baiting = leave a tempting item (USB) |
| Vishing / Smishing | voice call / SMS | vs Pharming = DNS redirect, no click |
| False negative | missed a real attack (most dangerous) | vs False positive = false alarm on benign traffic |
| IaaS / PaaS / SaaS | rent hardware / build apps / use software | more service = less customer control |
| Security Group / NACL | stateful / stateless | SG = instance level; NACL = subnet level |
| Golden / Silver ticket | forged TGT (whole domain) / forged TGS (one service) | Golden = Grand, Silver = Single |
| Vertical / Horizontal privesc | up to admin / sideways to another user | read the direction |
| CVE / CVSS / NVD | flaw ID / 0-10 score / the database | ATT&CK = attacker behavior, not a vuln list |
| AuthN / AuthZ | who you are / what you're allowed | authentication ≠ authorization |
| TTL fingerprint | Linux ≈ 64, Windows ≈ 128 | network device ≈ 255 |
| WEP / WPA / WPA2 / WPA3 | RC4+IV / RC4+TKIP / AES-CCMP / SAE | weak → strongest |
| Public vs private key | public encrypts & verifies / private decrypts & signs | public key can't decrypt its own ciphertext |


### Frameworks, orders & numbers

| Framework | In order / values |
|---|---|
| 5 Phases of Hacking | Recon → Scanning → Gaining Access → Maintaining Access → Clearing Tracks |
| CEH Methodology | Footprinting → Scanning → Enumeration → Vulnerability Analysis → System Hacking |
| Cyber Kill Chain (7) | Recon → Weaponize → Deliver → Exploit → Install → C2 → Actions on Objectives |
| Diamond Model | Adversary · Capability · Infrastructure · Victim |
| CVSS bands | 0 None · 0.1-3.9 Low · 4.0-6.9 Medium · 7.0-8.9 High · 9.0-10.0 Critical |
| OWASP Top 10 (2021) | A01 Broken Access Control (#1) · A03 Injection · A10 SSRF |
| Purdue (OT) | Levels 0-5: process → sensors → PLC → SCADA/HMI → IT → enterprise |
| System Hacking (GEMC) | Gaining → Escalating → Maintaining → Covering |


---

## Part B — Module-by-Module Review

### Module 01 — Introduction to Ethical Hacking

*The rules of the game: security principles, actors, phases and laws.*

**What to remember**

- CIA Triad: Confidentiality (no leaking), Integrity (no tampering), Availability (stays up). Spot which one a scenario breaks.
- 5 Phases of Hacking (order): Reconnaissance → Scanning → Gaining Access → Maintaining Access → Clearing Tracks.
- CEH Methodology (different list): Footprinting → Scanning → Enumeration → Vulnerability Analysis → System Hacking. Enumeration & Vuln-Analysis appear ONLY here, never in the 5 Phases.
- Hacker types: White (authorized), Black (criminal), Gray (no permission, no malice), Suicide, Script Kiddie, Hacktivist, State-sponsored.
- Testing: Black box = no knowledge, White box = full knowledge, Gray box = partial.
- Threat vs Vulnerability vs Risk vs Attack; Risk = Threat × Vulnerability × Impact.
- Cyber Kill Chain (7): Recon→Weaponize→Deliver→Exploit→Install→C2→Actions. MITRE ATT&CK = technique encyclopedia; Diamond Model = Adversary/Capability/Infrastructure/Victim.
- Laws: HIPAA=health, PCI-DSS=cards, GDPR=EU privacy, SOX=finance, ISO 27001=ISMS, CFAA=US unauthorized-access law.

> ⚠️ **Exam traps**
>
> - “Phase 4” is ambiguous — check which list: 5-Phases (Maintaining Access) vs Methodology (Vulnerability Analysis).
> - Unauthorized access = illegal even with good intent (CFAA).
> - Footprinting / Reconnaissance are the SAME Phase 1, just renamed across CEH versions.
>
> 🧠 **Mnemonic** — Kill Chain = “RWDEICA”: Recon, Weaponize, Deliver, Exploit, Install, C2, Actions.

### Module 02 — Footprinting & Reconnaissance

*Phase 1 — gather info on the target, mostly without being detected.*

**What to remember**

- Passive = no direct contact (WHOIS, OSINT, Shodan); Active = touches target (DNS zone transfer, traceroute, email tracking, social engineering).
- 7 categories: DNS, Network, Website, Email, Social Engineering, Competitive Intelligence, OSINT.
- Tool→job: theHarvester (emails/subdomains), Maltego (OSINT graph), Shodan (exposed devices), Recon-ng (framework), WHOIS (registration).
- Google Dorks: site:, filetype:, intitle:, inurl:.
- DNS records: A (IPv4), AAAA (IPv6), MX (mail), NS (name server), CNAME (alias), TXT (SPF/verify), SOA (zone admin).

> ⚠️ **Exam traps**
>
> - Footprinting is MOSTLY passive, but DNS zone transfer, traceroute, email tracking and social engineering are ACTIVE.
> - Full port scanning / banner grabbing belong to Module 03, not footprinting.
>
> 🧠 **Mnemonic** — “OSINT = Open-Source, no touch.” V13 adds AI-powered recon = automated OSINT correlation.

### Module 03 — Scanning Networks

*Phase 2 — find live hosts, open ports, services and OS (active).*

**What to remember**

- TCP 3-way handshake: SYN → SYN-ACK → ACK. Every scan type bends this.
- Nmap: -sS SYN/half-open (stealthy), -sT full connect (logged), -sU UDP, -sF FIN, -sX Xmas (FIN+PSH+URG), -sN NULL, -sA ACK (map firewall), -sI idle/zombie (source IP spoofed), -O OS, -sV version, -A aggressive.
- Host discovery: -sn (no port scan), -PR ARP, -PS/-PA/-PE/-PU pings. Evasion: -f fragment, -D decoys, -T0..T5 timing, -g source-port spoof.
- Port states: Open, Closed, Filtered (firewall blocks), Unfiltered.
- FIN/Xmas/NULL: closed port = RST; open/filtered = silence. Unreliable vs Windows.
- Banner grabbing: Active (telnet/netcat) vs Passive (Shodan).

> ⚠️ **Exam traps**
>
> - IP never in logs → idle/zombie scan (-sI). “Fast + stealthy from my own IP” → SYN scan (-sS).
> - Half-open = SYN scan (-sS), NOT TCP connect (-sT). Connect completes the handshake and gets logged.
> - TTL fingerprint: Linux ≈ 64, Windows ≈ 128, network device ≈ 255.
>
> 🧠 **Mnemonic** — Sneaky scans = FIN/Xmas/NULL → “silence means open.”

### Module 04 — Enumeration

*Phase 3 — actively connect and extract users, shares, services.*

**What to remember**

- Enumeration is ALWAYS active; no passive form. Intranet-focused (scanning = extranet-focused).
- 7 protocols + ports: NetBIOS 137-139, SNMP 161/162, LDAP 389, NTP 123, NFS 2049, SMTP 25, DNS 53.
- SNMP community strings: public = read-only, private = read-write (dangerous).
- NULL session / IPC$ = classic Windows anonymous-connection attack.
- Tools: enum4linux (NetBIOS/SMB all-in-one), snmp-check/snmpwalk, ldapsearch, dnsenum/Fierce, smtp-user-enum (VRFY/EXPN/RCPT).

> ⚠️ **Exam traps**
>
> - Default SNMP “public”/“private” string = misconfiguration vulnerability.
> - LDAP = 389 (not 636 unless LDAPS). Match port→protocol carefully.
>
> 🧠 **Mnemonic** — “NetBIOS=Windows, SNMP=devices, LDAP=directory, NTP=time, NFS=Linux shares, SMTP=email users, DNS=domain.”

### Module 05 — Vulnerability Analysis

*Find, classify and prioritize weaknesses (not exploit them yet).*

**What to remember**

- CVE = the ID/name of a flaw. CVSS = the 0-10 severity score. NVD = database (CVE+CVSS+affected products). ATT&CK = attacker behavior, not vulns.
- CVSS bands: 0 None · 0.1-3.9 Low · 4.0-6.9 Medium · 7.0-8.9 High · 9.0-10.0 Critical.
- Authenticated (credentialed) scan = deeper/accurate; Unauthenticated = attacker's outside view.
- Tools: Nessus (general), OpenVAS (free), Qualys (cloud), Nexpose (risk-priority), Nikto (web server).
- Vuln Management Life Cycle: Identify assets → Scan → Risk assess → Remediate → Verify → Monitor.

> ⚠️ **Exam traps**
>
> - 9.4 is Critical, not High. Critical starts at 9.0.
> - Use-After-Free is its own flaw (not Race Condition / Buffer Overflow).
> - Nikto is web-server specific; Nessus/OpenVAS/Qualys/Nexpose are broad scanners.
>
> 🧠 **Mnemonic** — “CVE = name card, CVSS = the grade on it, NVD = the filing cabinet.”

### Module 06 — System Hacking

*Gain access, escalate, maintain, cover tracks — the heart of the attack.*

**What to remember**

- 4 sub-phases (GEMC): Gaining → Escalating → Maintaining → Covering.
- SAM = local hashes; ntds.dit = domain (AD) hashes. LM weak/legacy, NTLMv2 ok, Kerberos strongest.
- Password categories (NAPO): Non-electronic, Active online, Passive online, Offline.
- Online cracking hits the live service (Hydra, Medusa). Offline cracks stolen hashes (Hashcat, John; Ophcrack=rainbow).
- Credential attacks: Pass-the-Hash (use NTLM hash), Pass-the-Ticket, Kerberoasting (TGS offline), AS-REP roasting, LLMNR/NBT-NS poisoning (Responder), Golden Ticket (forged TGT/krbtgt = domain), Silver Ticket (forged service TGS). Mimikatz pulls creds from memory.
- Privilege escalation: Vertical = up (user→admin); Horizontal = sideways to another user.
- Hide data: NTFS ADS (hidden stream in a file) vs Steganography (inside image/audio/video). Cover tracks: clearev, wevtutil, timestomping, auditpol, clear ~/.bash_history.

> ⚠️ **Exam traps**
>
> - Pass-the-Hash needs the HASH, not the plaintext password.
> - Golden = whole domain (TGT/krbtgt); Silver = single service (TGS).
> - Salting defeats rainbow tables.
>
> 🧠 **Mnemonic** — “Golden = Grand (domain), Silver = Single service.”

### Module 07 — Malware Threats

*Malicious software: viruses, worms, Trojans, ransomware, fileless, APT.*

**What to remember**

- Virus needs user action; Worm self-spreads (no user). Trojan = disguised, runs at user privilege. Ransomware encrypts for payment. Botnet = zombie army (C2).
- Polymorphic = changes appearance (encrypts), core logic same; Metamorphic = rewrites its whole code (harder).
- Dropper CARRIES the malware; Downloader FETCHES it from the internet.
- Components: Payload, Dropper, Downloader, Injector, Crypter, Packer, Obfuscator, Wrapper/Binder.
- Fileless = memory-only, abuses PowerShell/WMI = “Living off the Land.” APT = long-term, stealthy, targeted (nation-state).
- Analysis: Static = don't run (strings, PEiD, IDA/Ghidra, VirusTotal); Dynamic = run in sandbox (Cuckoo, Process Monitor, Wireshark). Sheep Dip = isolated test machine.
- Detection: signature (misses zero-day) vs heuristic/behavioral (catches unknown).

> ⚠️ **Exam traps**
>
> - “Spreads automatically / no user interaction” → WORM.
> - WannaCry used EternalBlue (SMB, port 445); best ransomware defense = offline backups.
> - Signature-based AV can't catch new/polymorphic malware — need heuristics.
>
> 🧠 **Mnemonic** — “Virus = Victim clicks, Worm = Wanders alone.” · “Dropper Delivers what it carries, Downloader Dials out.”

### Module 08 — Sniffing

*Capture packets in transit; works only on unencrypted traffic.*

**What to remember**

- Needs promiscuous mode (wired) + same network segment. Monitor mode = wireless (different!).
- HUB → passive sniffing works (everyone hears all). SWITCH → need ACTIVE techniques to force a leak.
- Passive = sends NO packets; Active = injects packets (noisier).
- MAC flooding (macof) overflows the CAM table → switch acts like a hub. Defense: Port Security.
- ARP poisoning: forged replies → MITM (ARP is stateless). Defense: Dynamic ARP Inspection.
- DHCP Starvation (empties IP pool) vs Rogue DHCP (fake gateway). Defense: DHCP Snooping.
- Plain-text ports: Telnet 23, HTTP 80, FTP 20/21, SMTP 25, POP3 110, IMAP 143, SNMP 161, TFTP 69.

> ⚠️ **Exam traps**
>
> - “Switched network” → the answer is almost always an ACTIVE technique.
> - MAC flooding makes a SWITCH behave like a HUB (not the reverse).
> - ARP is STATELESS — that's WHY spoofing works.
>
> 🧠 **Mnemonic** — “Hub Hears all, Switch Sends selective.”

### Module 09 — Social Engineering

*Hack the human: exploit trust and emotion; humans = weakest link.*

**What to remember**

- Cycle: Research → Select target → Build relationship → Exploit.
- 3 families: Human-based, Computer-based, Mobile-based.
- Spear phishing = specific person; Whaling = specific executive; Vishing = voice; Smishing = SMS; Pharming = DNS redirect (no click).
- Tailgating = NO consent; Piggybacking = WITH consent. Reverse SE = victim contacts the attacker.
- Tools: SET (attack), OhPhish (training), Netcraft/PhishTank (detect). V13 adds deepfake / voice cloning.

> ⚠️ **Exam traps**
>
> - Quid pro quo (offer a service for info) vs Baiting (leave a tempting item like a USB).
> - Phishing is social engineering — needs NO malware / exploit to succeed.
> - Best defense = user awareness training (not a firewall).
>
> 🧠 **Mnemonic** — “Voice=Vishing, SMS=Smishing, DNS=Pharming.” · “No patch for humans.”

### Module 10 — Denial of Service

*Attack AVAILABILITY — flood or exhaust so service goes down.*

**What to remember**

- DoS = 1 source; DDoS = many (botnet of zombies via C2).
- 3 categories: Volumetric (bps), Protocol (state), Application-layer (rps).
- SYN flood = PROTOCOL attack (half-open connections). Fix: SYN cookies.
- Smurf = spoofed victim IP + ICMP echo to broadcast. Fraggle = same idea over UDP.
- Amplification/reflection abuses stateless UDP: NTP 123, DNS 53, SSDP 1900 (small request → big reply).
- Ping of Death = oversized packet. Slowloris = LOW-bandwidth app-layer. Permanent DoS (phlashing) = bricks hardware.

> ⚠️ **Exam traps**
>
> - SYN flood is protocol, NOT volumetric bandwidth.
> - Smurf = ICMP; Fraggle = UDP. Don't swap.
> - Slowloris is low-bandwidth — opposite of a volumetric flood.
>
> 🧠 **Mnemonic** — “DoS kills the A” (Availability in CIA). “Smurf = Spoof + Broadcast + ICMP.”

### Module 11 — Session Hijacking

*Take over a LIVE session by stealing/predicting the session ID.*

**What to remember**

- Spoofing = NEW session (stolen creds); Hijacking = EXISTING active session. Auth already happened.
- Phases: Track → Desynchronize → Inject (inject needs the next sequence number).
- Application-level = session ID/cookie; Network-level = TCP sequence numbers.
- XSS steals cookies (defense: HttpOnly). CSRF rides a session (defense: anti-CSRF token + SameSite).
- Session fixation sets the ID BEFORE login → defense: regenerate ID AFTER login.
- Tools: Burp/bettercap/Caido (attack), Wireshark (detect). IPsec: Transport=payload, Tunnel=whole packet.

> ⚠️ **Exam traps**
>
> - Blind hijacking = inject without seeing replies (predict SEQ). RST hijacking ends the session, doesn't read it.
> - Hijacking needs an active session to already exist.
>
> 🧠 **Mnemonic** — “Spoof starts fresh, Hijack steals live.” · Cookie flags “HSS” = HttpOnly, Secure, SameSite.

### Module 12 — Evading IDS, Firewalls & Honeypots

*Defeat or slip past detection and filtering devices.*

**What to remember**

- Firewall = allow/block by rules; IDS = detect (alarm only); IPS = detect + block; Honeypot = decoy/trap.
- Alerts: True/False Positive, True/False Negative. FALSE NEGATIVE (missed attack) is the worst.
- Insertion = IDS accepts what the host rejects; Evasion = IDS rejects what the host accepts.
- Session splicing = many tiny packets to dodge signatures. Firewalking = uses TTL to map firewall rules.
- Tunneling: ICMP/ACK/HTTP/SSH/DNS. ACK tunneling works because firewalls trust “established” ACK packets.
- Firewall types: packet-filter (L3/4), circuit (L5), proxy (L7), stateful, NGFW (DPI+IPS). Tools: Snort/Suricata (IDS), Hping (crafting), Nessus (splicing).

> ⚠️ **Exam traps**
>
> - IDS detects only; IPS detects AND blocks.
> - Insertion vs Evasion — memorize the direction (who accepts what).
> - Any interaction with a honeypot is suspicious by design.
>
> 🧠 **Mnemonic** — “Fire Detects, IPS Protects, Honey Attracts.” · “Insertion = IDS eats extra; Evasion = IDS skips.”

### Module 13 — Hacking Web Servers

*Attack the SERVER and its configuration (not the app code).*

**What to remember**

- Module 13 = server + config; Module 14 = app code (SQLi/XSS).
- #1 cause of compromise = misconfiguration (default creds, verbose errors, directory browsing).
- Directory traversal = ../ escapes the web root; root cause = poor input validation.
- DNS hijacking = redirect via changed records; DNS amplification = UDP DDoS.
- HTTP response splitting = CRLF injection → cache poisoning + XSS. Only Apache uses .htaccess.
- IIS: HTTP.sys = kernel mode, w3wp.exe = user mode. Ports: HTTP 80, HTTPS 443, SSH 22, FTP 21, DNS 53, SMTP 25.

> ⚠️ **Exam traps**
>
> - Web SERVER (13) vs web APP (14) — attack surface differs.
> - Frontjacking = Nginx reverse-proxy misconfig, not a backend / DNS bug.
>
> 🧠 **Mnemonic** — “Doc root = content, Server root = control.” · “Dot-dot-slash escapes the stash.”

### Module 14 — Hacking Web Applications

*Attack the application CODE; OWASP Top 10 is the backbone.*

**What to remember**

- 3 layers: Presentation (client, NOT security) → Business logic → Database.
- OWASP Top 10 2021: A01 Broken Access Control (#1), A02 Crypto, A03 Injection, ... A10 SSRF.
- XSS: Stored (all users), Reflected (one victim via link), DOM (client-side). Fix: encode + HttpOnly + CSP.
- IDOR (web) = BOLA (API): change an ID to reach another user's object → enforce server-side authz.
- API vs Webhook: client pulls on request vs server pushes on event. APIs: REST (JSON), SOAP (XML/WSDL), GraphQL.
- Tools: Burp Suite, OWASP ZAP, SQLmap, Nikto, Dirb/Gobuster, Postman, BeEF.

> ⚠️ **Exam traps**
>
> - Client-side validation is NOT security — the server must validate.
> - A03 Injection now bundles SQLi + XSS + command injection.
> - SSRF = make the SERVER request a URL the attacker chooses.
>
> 🧠 **Mnemonic** — “Untrusted input + weak validation = most web attacks.” AuthN (who) ≠ AuthZ (allowed).

### Module 15 — SQL Injection

*Unsanitized input runs as SQL (OWASP A03).*

**What to remember**

- Cause: input concatenated into queries. Classic bypass: ' OR '1'='1 (always true), admin'-- (comment out password).
- Types: In-band (Error-based, UNION-based) · Blind (Boolean, Time-based) · Out-of-band (DNS/HTTP).
- UNION needs equal column count + compatible types. Enumerate via information_schema.tables/.columns.
- Time delays: MySQL SLEEP(), MSSQL WAITFOR DELAY, PostgreSQL pg_sleep(), Oracle DBMS_LOCK.SLEEP.
- OS commands: MSSQL xp_cmdshell, MySQL INTO OUTFILE. Tools: SQLmap, Havij, SQLninja.
- Comments: -- (most DBs), # (MySQL), /* */ (all).

> ⚠️ **Exam traps**
>
> - #1 fix = parameterized queries / prepared statements — NOT just escaping or a WAF.
> - Blind: Boolean = page changes; Time-based = response delay.
>
> 🧠 **Mnemonic** — “Parameterize, don't concatenate.” · “Delay = TRUE.”

### Module 16 — Hacking Wireless Networks

*Attack Wi-Fi: encryption weaknesses, rogue / evil-twin APs.*

**What to remember**

- Encryption ladder: WEP (RC4 + 24-bit IV, broken) → WPA (RC4+TKIP) → WPA2 (AES-CCMP + 4-way handshake) → WPA3 (SAE/Dragonfly).
- WPA2 risks: weak passphrase + KRACK. WPA3 benefit: SAE defeats offline dictionary attacks.
- Rogue AP = unauthorized; Evil Twin = fake AP mimicking a real SSID. Deauth forces a reconnect to capture the handshake (doesn't break crypto).
- SSID = network name, BSSID = AP MAC. Monitor mode captures Wi-Fi (≠ promiscuous).
- Aircrack flow: airmon-ng → airodump-ng → aireplay-ng (deauth) → aircrack-ng. WPS brute force = Reaver/Bully; detect with Wash.

> ⚠️ **Exam traps**
>
> - WEP=IV reuse, WPA=TKIP, WPA2=AES-CCMP, WPA3=SAE. Don't mix the ciphers.
> - Hidden SSID and MAC filtering are NOT real security.
> - Deauth does not decrypt — it just forces the handshake.
>
> 🧠 **Mnemonic** — “Weak, Better, Strong, Strongest” = WEP, WPA, WPA2, WPA3.

### Module 17 — Hacking Mobile Platforms

*Attack phones: device, network, cloud.*

**What to remember**

- Rooting = Android; Jailbreaking = iOS. Both break the sandbox + MDM.
- Android = open-source (weak vetting, custom ROMs); iOS = closed (strong vetting) but still attacked.
- Jailbreak types: tethered (needs PC each boot), semi-tethered, untethered (persistent).
- MitMo (Man-in-the-Mobile) = malware beating banking apps via overlay + SMS interception.
- Dangerous perms: READ_SMS (OTP theft), SEND_SMS (fraud), RECORD_AUDIO/CAMERA (spy). ADB = Android control (shell/pull/push/install).
- MDM ⊂ EMM ⊂ UEM. Tools: Drozer, APKTool, JADX, MobSF, Frida.

> ⚠️ **Exam traps**
>
> - Enterprise certificate abuse lets UNSIGNED apps run on iOS.
> - MDM can't stop zero-days and is bypassed on rooted/jailbroken devices.
>
> 🧠 **Mnemonic** — “Root = Jailbreak” (Android / iOS) · “SMS = money, mic = spy.”

### Module 18 — IoT & OT Hacking

*Connected gadgets (IoT) and industrial control (OT); OT hacks cause physical damage.*

**What to remember**

- IoT protocols: MQTT (pub/sub, broker, port 1883), CoAP (UDP, 5683). OT: Modbus (502, no auth), DNP3 (power).
- Device attacks: default creds, firmware tampering, JTAG/UART debug ports, chip-off.
- Mirai botnet = default creds over Telnet/SSH → DDoS. Flow: Scan → Infect → Control → Flood.
- OT components: PLC (control), RTU (remote), HMI (screen), SCADA (supervise). Purdue levels 0-5 (process → enterprise).
- Famous: Stuxnet (PLCs/centrifuges, 1st cyber-weapon), Triton (safety systems), BlackEnergy (power grid).
- Tools: Shodan/Censys (find devices — recon only), RouterSploit/Metasploit (exploit).

> ⚠️ **Exam traps**
>
> - Most OT protocols (Modbus, DNP3) have NO native security.
> - Firmware encryption alone is not enough — it must also be SIGNED.
> - Shodan is a search engine (recon), not an exploit tool.
>
> 🧠 **Mnemonic** — “SCADA supervises, PLC controls, HMI shows.” · “Signed + encrypted firmware.”

### Module 19 — Cloud Computing

*On-demand pooled IT; most breaches = misconfiguration + weak IAM.*

**What to remember**

- Models: IaaS (you manage OS+apps+data), PaaS (app code), SaaS (data only). More service = less control.
- Deployment: public, private, community, hybrid, multi-cloud.
- Shared responsibility: provider secures OF the cloud; customer secures IN the cloud and ALWAYS owns Data + IAM.
- Top threats: misconfiguration (public S3 buckets), insecure APIs (primary surface), account hijacking.
- Metadata attack = SSRF → metadata service → steal credentials. Escalation is policy/role-based (not kernel).
- Tools: Pacu, Prowler, ScoutSuite (attack/audit); GuardDuty/Defender (defend).

> ⚠️ **Exam traps**
>
> - Cloud is NOT secure by default; customer is liable for misconfiguration breaches.
> - Security Groups = STATEFUL; NACLs = STATELESS.
> - Encryption doesn't prevent breaches; logging is not optional.
>
> 🧠 **Mnemonic** — “Provider secures OF the cloud, customer secures IN the cloud.” · “SSRF → metadata → creds.”

### Module 20 — Cryptography

*Turn plaintext into ciphertext with a cipher + key. Goals: CIA + Non-repudiation.*

**What to remember**

- Symmetric = 1 shared key, fast, bulk (AES; problem = key distribution). Asymmetric = 2 keys (public+private), slow, trust (RSA).
- Public key encrypts & verifies; private key decrypts & signs.
- Symmetric: DES (broken), 3DES (slow), AES (standard, 128-bit block), RC4 (broken), ChaCha20 (modern). ECB insecure — prefer CBC/CTR/GCM.
- Asymmetric: RSA (all three), DH (key exchange only, MITM-vulnerable), DSA (signatures only), ECC (small keys, same strength). Ephemeral DH (DHE/ECDHE) = Perfect Forward Secrecy.
- Hashing = integrity, one-way. MD5/SHA-1 broken; SHA-2/SHA-3 secure. HMAC = hash + key. Salt stops rainbow tables. Password hashing: bcrypt/scrypt/PBKDF2.
- PKI: CA signs certs; chain Root→Intermediate→End; CRL (periodic) vs OCSP (real-time). Hybrid (TLS): asymmetric exchanges a session key, symmetric (AES) encrypts data.

> ⚠️ **Exam traps**
>
> - A public key CANNOT decrypt what it encrypted — only the matching private key can.
> - Diffie-Hellman = key exchange only (no encryption, no signing).
> - ECC = same security as RSA with smaller keys. Birthday attack = hash collisions; padding oracle = CBC.
>
> 🧠 **Mnemonic** — “Public encrypts, private decrypts; private signs, public verifies.” · “DES Dead, 3DES Tired, AES Alive.”

---

## Final exam-day reminders

- Read the whole scenario before the options — the keyword is usually buried in the last sentence.
- Eliminate two wrong answers first; CEH distractors are often a real term used in the wrong place.
- Port shows up? Map it instantly (53 DNS, 445 SMB, 389 LDAP, 3389 RDP, 161 SNMP, 1883 MQTT).
- "Stealthy / no logs" → idle scan or passive technique. "By itself / automatic" → worm. "Stateless" → ARP, UDP amplification, NACL.
- Defense questions: awareness training (social engineering), SYN cookies (SYN flood), salting (rainbow tables), parameterized queries (SQLi), DNSSEC (DNS poisoning), WPA3 / disable WPS (wireless).
- Manage time: ~125 questions in 4 hours ≈ under 2 minutes each. Flag and move on; never leave blanks.
