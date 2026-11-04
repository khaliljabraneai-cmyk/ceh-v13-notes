**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 13 — Hacking Web Servers**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Project documents (13 - Hacking web servers.md and 13.x study sheets) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

A web **server** is a computer system that stores, processes, and delivers web pages to clients over HTTP/HTTPS. **Hacking web servers means attacking the SERVER SOFTWARE** (Apache, IIS, Nginx) and its configuration **— not the web application code** (that's Module 14).

When CEH says **“Web Server Hacking”**, think: **Attack the web server software, OS, services, and configuration**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The restaurant picture</strong></p>
<p><strong>•</strong> The web SERVER is the kitchen + building (Apache/IIS/Nginx). The web APPLICATION is the recipe/menu logic (Module 14).</p>
<p><strong>•</strong> Module 13 = attack the kitchen: bad locks (misconfig), default keys (default creds), unpatched ovens (old software).</p>
<p><strong>•</strong> Browser asks → Server fetches → Server responds.</p></td>
</tr>
</tbody>
</table>

# **2) Web server components (high exam weight)**

| **Component** | **Purpose**                                 | **Exam hook**              |
|---------------|---------------------------------------------|----------------------------|
| Document Root | Stores the public HTML files for the domain | Public web files live here |
| Server Root   | Stores config, logs, executables            | Admin-level directory      |
| conf          | Configuration files                         | Controls server behavior   |
| logs          | Server logs                                 | Recon goldmine             |
| cgi-bin       | CGI scripts                                 | Command-execution risk     |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook</strong></p>
<p><strong>•</strong> Document Root = content · Server Root = control.</p>
<p><strong>•</strong> Virtual Hosting trinity: Name-based (many domains, 1 IP), IP-based (1 IP each), Port-based (different ports).</p>
<p><mark> <strong>Document Root serves content; Server Root manages the server.</strong></mark></p>
<p><mark><strong>Document Root</strong> = "Here are the website files for visitors."<br />
<strong>Server Root</strong> = "Here is the server's internal organization."<br />
<strong>conf</strong> = "These are the server's rules."<br />
<strong>logs</strong> = "This is the server's history."<br />
<strong>cgi-bin</strong> = "These are programs the server can execute</mark></p></td>
</tr>
</tbody>
</table>

# 

# **3) Why web servers get compromised**

A web server usually gets compromised because there is some weakness that the attacker can exploit.

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 38%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Root cause</strong></th>
<th><strong>Note</strong></th>
<th><strong>Simple idea</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Improper configuration</td>
<td>MOST common cause</td>
<td><mark>The server is installed, but the administrator <strong>configured it incorrectly</strong> (Unnecessary services are enabled, Permissions are too open, Default settings were not changed ..etc)</mark></td>
</tr>
<tr class="even">
<td>Weak / default credentials</td>
<td>Easy exploitation</td>
<td><mark>The administrator uses a weak password or never changes the default username/password.</mark></td>
</tr>
<tr class="odd">
<td>Unpatched software</td>
<td>Known public exploits</td>
<td><mark>The web server software is old and has known vulnerabilities.</mark></td>
</tr>
<tr class="even">
<td>Misconfigured SSL/TLS</td>
<td>MITM risk</td>
<td><p><mark>SSL/TLS protects communication between the browser and web server.</mark></p>
<p><mark>If it is configured incorrectly, the security of the communication can be weakened and may increase the risk of MITM (Man-in-the-Middle) attacks (Bad encryption configuration).</mark></p></td>
</tr>
<tr class="odd">
<td>Third-party plugins</td>
<td>Supply-chain risk</td>
<td><p><mark>A web server may use software, modules, plugins, or components developed by another company.</mark></p>
<p><mark>If that third-party component has a vulnerability, attackers may exploit it.</mark></p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook</strong></p>
<p><strong>•</strong> Order of blame: Config &gt; Passwords &gt; Patching &gt; Crypto &gt; Plugins.</p></td>
</tr>
</tbody>
</table>

# **4) The 3 web servers (know the architecture clue)**

Apache, IIS, and Nginx are web server software

| **Server**          | **Model / key fact**                                                                         | **Exam clue**                                |
|---------------------|----------------------------------------------------------------------------------------------|----------------------------------------------|
| Apache              | Process/module-centric **(mod_ssl, mod_auth, mod_rewrite, mod_proxy)**                       | Modules misconfigured = danger               |
| IIS (**Microsoft**) | **Kernel** + user mode: **HTTP.sys** (kernel) → WAS → **w3wp.exe** (user). No .htaccess      | Privilege escalation; HTTP.sys = kernel      |
| Nginx               | **Single-threaded, event-driven, non-blocking; reverse proxy / load balancer. No .htaccess** | Config error hits ALL workers; DoS amplified |

- **Apache modules:** mod_ssl (TLS), mod_auth (authn/authz), mod_rewrite (URL rewrite), mod_proxy (forward/gateway).

- **IIS request path:** Client → HTTP.sys (kernel) → WAS (reads ApplicationHost.config) → WWW service → w3wp.exe (user mode) → response.

# **5) Web server attacks (the master list — very high yield)**

| Attack                                                   | How it works — very simple                                                        | What to remember for exam                    |
|----------------------------------------------------------|-----------------------------------------------------------------------------------|----------------------------------------------|
| Web Server Misconfiguration                              | Uses bad server settings, default files, directory browsing, verbose errors, etc. | Bad configuration → exposed server           |
| **Directory / Path Traversal** | Uses ../ to move outside the web root and read files                              | ../ → outside web root                       |
| **DNS Server Hijacking**       | Attacker changes DNS mappings so users are sent to a fake server                  | Change DNS → fake site                       |
| **DNS Amplification**          | Small spoofed DNS request causes a large response to flood the victim             | Small request → BIG reply → DDoS             |
| **HTTP Response Splitting**    | Injects CRLF into HTTP headers to manipulate the response                         | CRLF → split/manipulate response             |
| Web Cache Poisoning            | Puts malicious content into a cache so many users receive it                      | Poison cache → many users affected           |
| SSH Brute Force                                          | Repeatedly guesses SSH usernames/passwords to get remote access                   | TCP 22 → guess creds → shell                 |
| FTP Brute Force                                          | Repeatedly guesses FTP usernames/passwords to gain FTP access                     | FTP → guess creds → access                   |
| HTTP/2 Continuation Flood                                | Sends endless HTTP/2 continuation frames and exhausts server resources            | Endless frames → CPU/memory exhaustion → DoS |
| **Frontjacking**               | Uses CRLF + Host-header manipulation against a reverse proxy                      | Nginx reverse proxy + headers                |
| Website Defacement                                       | After compromising the server, attacker changes the website's appearance/content  | Compromise → change website                  |
| SSRF                                                     | Tricks the server into sending requests to internal systems                       | Trick server → access internal systems       |

# **6) Four attacks the exam loves (deeper)**

## **Directory / Path traversal**

- Attacker sends ../../ in the URL to climb out of the web root and read system files (e.g., /etc/passwd).

- **Fix:** validate/sanitize input; never trust user-supplied file paths.

<!-- -->

- **Exam trigger:** If you see **../ + accessing files outside the web directory**, think **Path Traversal**.

## **DNS hijacking vs DNS amplification (don't confuse)**

|        | **DNS hijacking**                    | **DNS amplification**            |
|--------|--------------------------------------|----------------------------------|
| Goal   | Redirect users to a fake site        | Flood a victim (DDoS)            |
| Method | Change DNS records on the server     | Spoofed small query → huge reply |
| Result | Silent redirection (URL looks right) | Service unavailability           |

- **Hijacking = attacker changes the DNS → redirects users.**

- **Amplification = attacker uses DNS to create BIG traffic → floods victim.**

|                   | **🔴 DNS Hijacking**                                         | **🔵 DNS Amplification**                         |
|-------------------|--------------------------------------------------------------|--------------------------------------------------|
| **Attacker does** | **Changes DNS records**                                      | **Sends a fake DNS request**                     |
| **How?**          | Changes example.com → attacker's IP                          | Uses **victim's IP as the fake sender**          |
| **Then what?**    | User asks: “Where is example.com?” → DNS gives attacker's IP | DNS server sends a **large reply to the victim** |
| **Goal**          | Send users to a **fake website**                             | **Flood the victim** with traffic                |
| **Result**        | User reaches the **wrong website**                           | Victim's service can **go down**                 |
| **Key clue**      | **Change DNS → redirect**                                    | **Small request → big reply → DDoS**             |

## **HTTP response splitting → cache poisoning**

- Inject CRLF (carriage-return + line-feed) into a header to add a second, fake HTTP response.

- If a cache stores it, every later user gets the poisoned content = web cache poisoning.

**Attacker**

**↓**

**Injects CRLF**

**↓**

**Creates fake HTTP response**

**↓**

**Cache stores it**

**↓**

**Other users visit website**

**↓**

**❌ They receive poisoned content**

## **Frontjacking**

- CRLF → malicious Host header → misconfigured Nginx reverse proxy reroutes to attacker's server → fake content shown in the victim's browser.

- **Exam trap:** it is NOT a backend bug, NOT client-side only, NOT DNS-based. It's a reverse-proxy misconfig.

**Attacker**

**↓**

**CRLF + fake Host header**

**↓**

**Misconfigured Nginx**

**↓**

**"Okay, I'll send it there"**

**↓**

**Attacker's server**

**↓**

**Victim sees fake content**

# **7) Attack methodology & tools**

- Information gathering (Whois, robots.txt), footprinting/banner grabbing, mirroring, vulnerability scanning, then exploit.

| **Tool**           | **Purpose**                                           |
|--------------------|-------------------------------------------------------|
| Nmap               | Port scan, service/version detection, banner grabbing |
| Nikto              | Web server vulnerability scanner                      |
| THC Hydra / Ncrack | Password brute forcing (SSH 22, FTP, HTTP login)      |
| Nessus / Acunetix  | Vulnerability scanning                                |
| Metasploit         | Exploitation framework                                |
| Wireshark          | Traffic analysis / detection                          |

| **Step** | **What attacker does** | **Example** |
|------------------------------------|--------------------------------------------------|---------------------------------------|
| 1\. Information gathering          | Collect information about the website/server     | Whois, robots.txt                     |
| 2\. Footprinting                   | Learn about the server and software              | Banner grabbing                       |
| 3\. Scanning                       | Find open ports and services                     | Nmap                                  |
| 4\. Vulnerability scanning         | Find known weaknesses                            | Nikto, Nessus                         |
| 5\. Brute force                    | Try many passwords                               | Hydra, Ncrack                         |
| 6\. Exploitation                   | Use a vulnerability to gain access               | Metasploit                            |
| 7\. Traffic analysis               | Look at network traffic                          | Wireshark                             |

# **8) Ports / protocols to remember**

| **Port** | **Service** | **Relevance**                                  |
|----------|-------------|------------------------------------------------|
| 80       | HTTP        | Web traffic; HTTP/2 floods, response splitting |
| 443      | HTTPS       | TLS; misconfigured SSL = MITM                  |
| 22       | SSH         | SSH brute force → shell                        |
| 21       | FTP         | FTP brute force (plaintext)                    |
| 53       | DNS         | DNS hijacking & amplification (UDP)            |
| 25       | SMTP        | Mail-related server attacks                    |

# **9) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> A tester browses to http://site/download?file=../../../../etc/passwd.</p>
<p><strong>•</strong> The server doesn't validate the path and returns the system password file.</p>
<p><strong>•</strong> This is a directory/path traversal attack (input-validation failure).</p>
<p><strong>•</strong> Escalation: read config files → find DB creds → deeper compromise.</p>
<p><strong>•</strong> Fix: input validation, canonicalize paths, run server with least privilege, disable directory browsing.</p></td>
</tr>
</tbody>
</table>

# **10) Countermeasures**

- Harden config: remove default accounts/samples, disable directory browsing, hide version banners, turn off verbose errors.

- Patch the server + modules promptly; disable unused services/modules.

- Strong auth + MFA; rate-limit and lock out brute force (SSH/FTP).

- Validate all input (stops traversal, CRLF/response splitting).

- Proper TLS config; secure DNS (DNSSEC) against hijacking; limit recursion against amplification.

- WAF in front of the server; run as least-privilege user; segment the server in a DMZ.

# **11) Related concepts**

| **Concept**           | **One-line meaning**                                                 |
|-----------------------|----------------------------------------------------------------------|
| Web server vs web app | Module 13 = server software/config; Module 14 = app code (SQLi, XSS) |
| CRLF injection        | Inserting carriage-return/line-feed to forge headers                 |
| Reverse proxy         | Server (e.g., Nginx) that forwards client requests to backends       |
| Website defacement    | Visible content change after compromise                              |
| SSRF                  | Server tricked into requesting internal/other resources              |

# **12) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Web SERVER hacking (13) attacks the server/config; web APP hacking (14) attacks the code (SQLi/XSS).</p>
<p><strong>•</strong> DNS hijacking = redirect (change records). DNS amplification = DDoS (small request, big reply).</p>
<p><strong>•</strong> Directory traversal = '../' to escape the web root; root cause = input validation.</p>
<p><strong>•</strong> HTTP response splitting needs CRLF injection; it enables cache poisoning + XSS.</p>
<p><strong>•</strong> Frontjacking = Nginx reverse-proxy misconfig, NOT a backend or DNS bug.</p>
<p><strong>•</strong> IIS: HTTP.sys = kernel mode, w3wp.exe = user mode. Nginx/IIS don't use .htaccess (that's Apache).</p>
<p><strong>•</strong> 'Most common cause of web server compromise' = misconfiguration.</p></td>
</tr>
</tbody>
</table>

# **13) Mnemonics**

- **Why compromised:** 'Config \> Passwords \> Patching \> Crypto \> Plugins.'

- **Components:** 'Doc root = content, Server root = control.'

- **Virtual hosting:** 'Name, IP, Port' trinity.

- **DNS pair:** 'Hijack = redirect, Amplify = flood.'

- **Traversal:** 'Dot-dot-slash escapes the stash (web root).'

- **IIS layers:** 'Kernel HTTP.sys, user w3wp.'

# **14) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Web server = stores/serves pages over HTTP/HTTPS. Module 13 = server + config, not app code.</p>
<p><strong>•</strong> Servers: Apache (modules), IIS (HTTP.sys kernel + w3wp user), Nginx (event-driven reverse proxy). Only Apache uses .htaccess.</p>
<p><strong>•</strong> #1 cause of compromise = misconfiguration (default creds, verbose errors, dir browsing).</p>
<p><strong>•</strong> Directory traversal = ../ escapes web root; cause = input validation.</p>
<p><strong>•</strong> DNS hijacking = redirect via changed records; DNS amplification = UDP DDoS (small query, big reply).</p>
<p><strong>•</strong> HTTP response splitting = CRLF in headers → web cache poisoning + XSS.</p>
<p><strong>•</strong> Frontjacking = CRLF + Host header on misconfigured Nginx reverse proxy.</p>
<p><strong>•</strong> SSH brute force on TCP 22 (Hydra/Ncrack) precedes lateral movement.</p>
<p><strong>•</strong> HTTP/2 continuation flood = END_HEADERS never set → memory/CPU exhaustion DoS.</p>
<p><strong>•</strong> Ports: HTTP 80, HTTPS 443, SSH 22, FTP 21, DNS 53, SMTP 25.</p>
<p><strong>•</strong> Defense: harden config, patch, MFA + lockout, validate input, TLS/DNSSEC, WAF, least privilege.</p></td>
</tr>
</tbody>
</table>

*End of Module 13 — Hacking Web Servers. Next up: quiz this module, or move to Module 14 (Hacking Web Applications).*
