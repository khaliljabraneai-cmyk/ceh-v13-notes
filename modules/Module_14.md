**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 14 — Hacking Web Applications**

*Beginner-friendly · Scenario-ready · What to memorize cold · HIGH-ROI module*

Sources: Project documents (14.1–14.5 study sheets) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted. (SQL injection has its own deep dive in Module 15.)

# **1) Definition**

**A web application** is **software** that runs in a browser and acts as the **interface between users and web servers** over HTTP/HTTPS. Hacking web applications means **attacking the APP CODE and logic** (input handling, auth, sessions) — not the server software (that was Module 13).

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>The restaurant picture (continued)</strong></p>
<p><strong>•</strong> Module 13 attacked the kitchen/building (the server). Module 14 attacks the MENU and RECIPES — the application logic.</p>
<p><strong>•</strong> The waiter takes your order (input). If the kitchen blindly trusts a malicious order, bad things happen (injection, XSS).</p>
<p><strong>•</strong> Golden rule of this module: user input + poor validation = most web attacks.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **2) How web apps work & why they're vulnerable**

Flow: URL → HTTP request → Web server → Application server (logic) → Database → Response.

## **3-layer architecture**

| **Layer**      | **Purpose**                                              | **Exam note**                          |
|----------------|----------------------------------------------------------|----------------------------------------|
| Presentation   | Browser UI, HTML/CSS/JavaScript                          | Client-side validation is NOT security |
| Business logic | Web + application server (Java, PHP, Python, .NET, Node) | Where the real logic runs              |
| Database       | DBMS stores data (MySQL, MSSQL, Oracle)                  | Web apps EXPOSE the DB                 |

- **Why vulnerable:** complex architecture, many integration points, user-controlled input, third-party components, rapid dev cycles, poor input validation.

# **3) The 5 module objectives**

#  To understand what you must learn in the Web Application Hacking module.

# 

| **\#** | **Objective**                           |                                                                        |
|--------|-----------------------------------------|------------------------------------------------------------------------|
| 1      | Understand web application **concepts** | Explain how web applications work and their architecture.              |
| 2      | Identify web application **threats**    | Recognize common vulnerabilities and attacks.                          |
| 3      | Understand the hacking **methodology**  | Describe the steps used to assess a web application.                   |
| 4      | Understand web **APIs** and webhooks    | Explain how applications communicate and exchange data.                |
| 5      | Apply **security testing** techniques   | Understand how to identify and assess web application vulnerabilities. |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Memory hook</strong></p>
<p><strong>•</strong> 'C-T-M-A-S' → Concepts, Threats, Methodology, APIs, Security testing.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **4) OWASP Top 10 (2021) — the exam centerpiece**

**OWASP = Open Worldwide Application Security Project.** OWASP is a nonprofit organization that provides resources, standards, and tools to improve application security.

Memorize all ten in order. This is the single highest-yield list in the web modules.

**OWASP Top 10 — Web Application Security Risks**

| **Rank** | **Code** | **Vulnerability**                                                                                            | **One-line meaning**                                   |
|----------|----------|--------------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| 1        | A01      | Broken Access Control                                                                                        | Users do actions they shouldn't (IDOR, force browsing) |
| 2        | A02      | Cryptographic Failures                                                                                       | Weak/missing crypto exposes data                       |
| 3        | A03      | Injection                                                                                                    | Untrusted input run as code (SQLi, XSS, command)       |
| 4        | A04      | Insecure Design                                                                                              | Flaw in the design, not a coding bug                   |
| 5        | A05      | Security Misconfiguration                                                                                    | Default configs, verbose errors, open settings         |
| 6        | A06      | Vulnerable/Outdated Components                                                                               | Old libraries/frameworks with known CVEs               |
| 7        | A07      | Identification & Auth Failures                                                                               | Weak login, no MFA, session issues                     |
| 8        | A08      | Software & Data Integrity Failures                                                                           | Insecure CI/CD, unsigned updates                       |
| 9        | A09      | Logging & Monitoring Failures                                                                                | No audit trail, attacks go unseen                      |
| 10       | A10      | SSRF (SSRF happens when an attacker tricks a web server into sending requests to an unintended destination.) | Server tricked into fetching attacker's URL            |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Memory hook</strong></p>
<p><strong>•</strong> Access → Crypto → Injection → Design → Config → Components → Auth → Integrity → Logging → SSRF.</p>
<p><strong>•</strong> A01 Broken Access Control is now #1 (moved up from 2017).</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **5) Key web application attacks (high yield)**

## **Cross-Site Scripting (XSS) — 3 types**

**XSS happens when an attacker makes a website run malicious JavaScript in another user's browser.**

| **Type**            | **How it works**                                             | **Clue**                                 |
|---------------------|--------------------------------------------------------------|------------------------------------------|
| Stored (persistent) | Malicious script saved on the server, runs for every visitor | Comment/profile field; affects all users |
| Reflected           | Script in a URL/link, bounced back to one victim             | Sent via a crafted link                  |
| DOM-based           | Script manipulates the page via the browser's DOM            | Happens entirely client-side in JS       |

- **XSS goal/defense:** steal cookies/sessions (document.cookie). Fix: input validation, output encoding, HttpOnly cookies.

## **Other core attacks**

| **Attack**              | **Core idea**                                             | **Defense**                                 |
|-------------------------|-----------------------------------------------------------|---------------------------------------------|
| SQL Injection (SQLi)    | Inject SQL via input to read/alter the DB (see Module 15) | Parameterized queries / prepared statements |
| CSRF                    | Trick a logged-in user's browser into an unwanted action  | Anti-CSRF tokens, SameSite cookies          |
| Command injection       | Input run as an OS command                                | Validate input, avoid shell calls           |
| IDOR                    | Change an ID/parameter to access another user's data      | Server-side authorization checks            |
| File upload (web shell) | Upload a malicious script and run it                      | Validate type/size, no execute perms        |
| SSRF                    | Make the server request internal systems                  | Allowlist outbound destinations             |

# **6) Web application hacking methodology (8 phases)**

| **\#** | **Phase**              | **What happens**                                                          |
|--------|------------------------|---------------------------------------------------------------------------|
| 1      | Information gathering  | Domain, IP, tech stack (Whois, Nslookup, Google dorks)                    |
| 2      | Web app footprinting   | Identify server, OS, CMS, framework (Wappalyzer, WhatWeb) — passive recon |
| 3      | Vulnerability scanning | Find known vulns (Nikto, Nessus, Acunetix) — scanner ≠ exploit            |
| 4      | Web app enumeration    | Dirs, files, params, roles, APIs (Dirb, Gobuster, wfuzz) — ACTIVE         |
| 5      | Exploitation           | Launch the actual attack (SQLi, XSS, etc.)                                |
| 6      | Post-exploitation      | Escalate, pivot, persist                                                  |
| 7      | Maintaining access     | Keep a foothold                                                           |
| 8      | Covering tracks        | Erase evidence                                                            |

**CEH memory line: Learn → Identify → Scan → Discover → Exploit → Assess → Maintain → Hide.**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Exam traps (methodology)</strong></p>
<p><strong>•</strong> Footprinting = PASSIVE recon (not scanning). Enumeration = ACTIVE. Scanner ≠ exploit.</p>
<p><strong>•</strong> Memory hook: 'I F S E E P M C' = Info, Footprint, Scan, Enumerate, Exploit, Post, Maintain, Cover.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **7) Web APIs & webhooks (v13 focus)**

# **The key idea: An API lets applications communicate. A webhook automatically sends a message when an event happens.**

<table>
<colgroup>
<col style="width: 19%" />
<col style="width: 80%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>API type</strong></th>
<th><strong>Key fact</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>REST</td>
<td>Uses HTTP requests to read or modify data. Commonly uses <strong>JSON</strong>— most common</td>
</tr>
<tr class="even">
<td>SOAP</td>
<td><p>Uses a strict message format based on XML, with defined rules and standards.</p>
<p>XML only, strict, uses WSDL + WS-Security <strong>//</strong> <strong>XML</strong></p></td>
</tr>
<tr class="odd">
<td>GraphQL</td>
<td>Client defines exactly what data it wants // <strong>Query + JSON response</strong></td>
</tr>
</tbody>
</table>

- **REST methods:** GET (read), POST (create), PUT (update), PATCH (partial), DELETE (remove).

- **API auth:** API keys → Basic auth → OAuth 2.0 → JWT (signed JSON tokens).

**Easy example**

Imagine your company ERP has an API for retrieving invoices.

1.  API Key: The application provides its API key.

2.  Basic Auth: The application provides a username and password.

3.  OAuth 2.0: The application obtains permission and uses an access token.

4.  JWT: The application sends a signed token that the server can validate.

These are different mechanisms, not four mandatory steps in a sequence. An API may use one or combine compatible mechanisms.

## **Top API vulnerabilities (OWASP API)**

- BOLA (Broken Object Level Authorization — the API version of IDOR), broken user auth, excessive data exposure, no rate limiting, mass assignment, injection, improper assets management.

These are common security problems that attackers may exploit in APIs.

| **Vulnerability**                 | **Very simple meaning**                                                          | **Memory clue**         |
|-----------------------------------|----------------------------------------------------------------------------------|-------------------------|
| BOLA                              | A user accesses another user's data by changing an object ID.                    | Someone else's data     |
| Broken Authentication             | The API fails to properly verify a user's identity.                              | Who are you?            |
| Excessive Data Exposure           | The API returns more information than necessary.                                 | Too much data           |
| Unrestricted Resource Consumption | The API allows excessive requests that consume resources.                        | Too many requests       |
| Mass Assignment                   | A user changes a field they should not be allowed to modify, such as their role. | Change protected fields |
| Injection                         | Malicious input changes a database query or system command.                      | Input changes execution |
| Improper Asset Management         | Old, forgotten, or undocumented API versions remain exposed.                     | Forgotten APIs          |

## **API vs Webhook**

| **Feature**   | **API**           | **Webhook**        |
|---------------|-------------------|--------------------|
| Communication | Client PULLS data | Server PUSHES data |
| Trigger       | Request-based     | Event-based        |
| Direction     | Bidirectional     | One-way            |

- **Webhook flow:** Event → Trigger → HTTP POST → Receiver processes. Risks: no auth, payload tampering, replay, data leakage.

# **8) Tools (know tool → purpose)**

# CEH V13 — Web application security tools

| Tool       | Main purpose                               | When to use it — simple example                                           | Free / Open source?                                        |
|------------|--------------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------|
| Burp Suite | Intercept HTTP/HTTPS requests              | Inspect a login request and test how the website handles modified inputs  | Community: Free, closed source; Pro: Paid                  |
| OWASP ZAP  | Web application vulnerability scanning     | Scan your own test website for potential vulnerabilities                  | Free and open source                                       |
| SQLmap     | Automated SQL injection testing            | Check whether a vulnerable search field allows SQL injection              | Free and open source. GitHub                               |
| Nikto      | Web server vulnerability scanning          | Check a test server for outdated software and insecure configurations     | Free and open source                                       |
| Acunetix   | Automated web application scanning         | Scan a company's website for common security weaknesses                   | Commercial / Paid; limited capabilities may be free        |
| Dirb       | Find hidden directories and files          | Look for paths such as /admin or /backup                                  | Free and open source                                       |
| Gobuster   | Discover directories, files and subdomains | Find directories that are not linked from the homepage                    | Free and open source                                       |
| Wfuzz      | Fuzz web inputs and parameters             | Test how a search parameter responds to different inputs                  | Free and open source                                       |
| Postman    | Send and inspect API requests              | Send a REST request and examine its JSON response                         | Free tier; proprietary, paid plans available               |
| SoapUI     | Test SOAP and REST APIs                    | Send a SOAP XML request and check the response                            | Open-source edition is free; commercial ReadyAPI available |
| Insomnia   | Create and test API requests               | Test API endpoints, headers, authentication and responses                 | Free tier; proprietary, paid plans available               |
| BeEF       | Browser security testing and exploitation  | In an authorized lab, assess the risks of a compromised or hooked browser | Free and open source                                       |

# **9) Security testing techniques**

- **Testing types:** Black-box (no knowledge), White-box (full source), Gray-box (partial). 'Black=Blind, White=All, Gray=Some.'

- Input validation testing (SQLi/XSS/command), authentication testing, session management testing, authorization testing (IDOR, privilege escalation).

- **AuthN vs AuthZ:** Authentication = who you are; Authorization = what you're allowed to do. 'AuthN ≠ AuthZ.'

**1. Testing types: Black-box, White-box, Gray-box**

| Type      | Simple meaning                                                  | Example                                                                            | Remember |
|-----------|-----------------------------------------------------------------|------------------------------------------------------------------------------------|----------|
| Black-box | Tester has no internal knowledge                                | Test a website like an external attacker                                           | Blind    |
| White-box | Tester has full internal knowledge, often including source code | Review source code and test the application                                        | All      |
| Gray-box  | Tester has partial knowledge or access                          | Test an application with a normal user account and limited technical documentation | Some     |

**2. Security testing techniques**

| Technique                    | What do you test?                                 | Simple example                                                 |
|------------------------------|---------------------------------------------------|----------------------------------------------------------------|
| Input validation testing     | Whether inputs are handled safely                 | Enter unexpected characters to test for SQL injection or XSS   |
| Authentication testing       | Whether identity verification works correctly     | Test whether invalid passwords are rejected                    |
| Session management testing   | Whether sessions and session tokens are protected | Check whether a logged-out session token still works           |
| Authorization testing        | Whether users can access only permitted resources | Try accessing another user's record to test for IDOR           |
| Privilege escalation testing | Whether a user can obtain unauthorized privileges | Check whether a normal user can access administrator functions |

# **10) Ports / protocols**

| **Port** | **Service** | **Relevance**                   |
|----------|-------------|---------------------------------|
| 80       | HTTP        | All web app traffic             |
| 443      | HTTPS       | Encrypted web app traffic       |
| 3306     | MySQL       | DB behind the app (SQLi target) |
| 1433     | MSSQL       | DB behind the app               |
| 8080     | HTTP-alt    | Proxies / app servers           |

# **11) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> A user profile field lets an attacker save &lt;script&gt;...&lt;/script&gt; that steals cookies.</p>
<p><strong>•</strong> Every visitor who views the profile runs the script → their session cookies are sent to the attacker.</p>
<p><strong>•</strong> This is STORED (persistent) XSS — it affects all viewers, not just one.</p>
<p><strong>•</strong> The attacker then reuses a stolen cookie to hijack a session (link to Module 11).</p>
<p><strong>•</strong> Fix: validate/encode output, set HttpOnly on cookies, use a Content Security Policy (CSP).</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **12) Countermeasures**

- Validate & sanitize ALL input server-side (client-side validation is not security).

- Parameterized queries/prepared statements (SQLi); output encoding + CSP + HttpOnly (XSS).

- Anti-CSRF tokens + SameSite cookies; strong auth + MFA; proper session management.

- Enforce server-side authorization (stops IDOR/BOLA); least privilege on the DB.

- Patch components; secure config; rate limiting & logging; WAF in front (not a silver bullet).

# **13) Related concepts**

| **Concept**             | **One-line meaning**                                          |
|-------------------------|---------------------------------------------------------------|
| Web server vs web app   | Module 13 = server/config; Module 14 = app code (this module) |
| IDOR vs BOLA            | Same flaw: IDOR in web apps, BOLA in APIs                     |
| AuthN vs AuthZ          | Who you are vs what you may do                                |
| Stored vs Reflected XSS | Saved on server (all users) vs bounced via link (one user)    |
| API vs Webhook          | Pull on request vs push on event                              |

# **14) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Client-side validation is NOT security — the server must validate.</p>
<p><strong>•</strong> A01 (Broken Access Control) is the #1 OWASP risk in 2021.</p>
<p><strong>•</strong> Injection (A03) now bundles SQLi + XSS + command injection together.</p>
<p><strong>•</strong> Stored XSS = all visitors; Reflected XSS = one victim via link; DOM XSS = client-side only.</p>
<p><strong>•</strong> Footprinting = passive; Enumeration = active; a scanner finds, it does not exploit.</p>
<p><strong>•</strong> IDOR (web) = BOLA (API) — change an ID to reach another user's object.</p>
<p><strong>•</strong> API = client pulls (request); Webhook = server pushes (event).</p>
<p><strong>•</strong> SSRF (A10) = make the SERVER request a URL the attacker chooses.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **15) Mnemonics**

- **Objectives:** 'C-T-M-A-S' → Concepts, Threats, Methodology, APIs, Security.

- **Methodology:** 'I F S E E P M C.'

- **OWASP:** 'Access, Crypto, Injection, Design, Config, Components, Auth, Integrity, Logging, SSRF.'

- **REST verbs:** 'Get, Post, Put, Patch, Delete' = G-P-P-P-D.

- **Core rule:** 'Untrusted input + weak validation = most web attacks.'

# **16) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Web app = browser software over HTTP/HTTPS; Module 14 = app CODE, not server.</p>
<p><strong>•</strong> 3 layers: Presentation (client, not security) → Business logic → Database.</p>
<p><strong>•</strong> OWASP Top 10 2021 in order; A01 Broken Access Control is #1; A03 Injection; A10 SSRF.</p>
<p><strong>•</strong> XSS: Stored (all users), Reflected (one via link), DOM (client-side). Fix: encode + HttpOnly + CSP.</p>
<p><strong>•</strong> SQLi fix = parameterized queries (deep dive in Module 15). CSRF fix = anti-CSRF token + SameSite.</p>
<p><strong>•</strong> IDOR (web) = BOLA (API): enforce server-side authorization.</p>
<p><strong>•</strong> Methodology: Info → Footprint (passive) → Scan → Enumerate (active) → Exploit → Post → Maintain → Cover.</p>
<p><strong>•</strong> APIs: REST (HTTP, JSON), SOAP (XML, WSDL, WS-Security), GraphQL. Auth: keys, Basic, OAuth2, JWT.</p>
<p><strong>•</strong> API vs Webhook: pull on request vs push on event (Event → Trigger → POST → Process).</p>
<p><strong>•</strong> Tools: Burp Suite, OWASP ZAP, SQLmap, Nikto, Acunetix, Dirb/Gobuster, Postman, BeEF.</p>
<p><strong>•</strong> Testing: Black=Blind, White=All, Gray=Some. AuthN ≠ AuthZ.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

*End of Module 14 — Hacking Web Applications. Next up: quiz this module, or move to Module 15 (SQL Injection — the deep dive).*
