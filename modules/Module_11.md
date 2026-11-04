**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 11 — Session Hijacking**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Google Drive (CEH V13 Master Study Guide) + Project documents (11 - Session hijacking.md). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

**Session hijacking** is taking control of a **valid**, already-**active** session between a user and a server — by stealing or predicting the **session ID/token** — so the attacker is treated as the logged-in user.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The cinema-ticket picture</strong></p>
<p><strong>•</strong> You show your ticket at the door and get a wristband (the session ID). After that, nobody re-checks your ticket — the wristband is enough.</p>
<p><strong>•</strong> A session hijacker steals or copies your wristband and walks in as 'you'.</p>
<p><strong>•</strong> Why it works: most systems authenticate ONCE at the start, then trust the session ID.</p></td>
</tr>
</tbody>
</table>

# **2) Simple explanation**

- **Steal the token, not the password:** the attacker needs the valid session ID, not your login.

- **Authentication happens once:** after login, the server trusts the session ID for every request.

- **Spoofing vs Hijacking:** spoofing = start a NEW session with stolen creds. Hijacking = seize an EXISTING active session.

# **3) Why session hijacking succeeds**

- No account lockout or invalidation of old session IDs.

- Weak session-ID generation (short, sequential, predictable).

- Insecure handling of session IDs (sent in clear text, in URLs).

- Indefinite session timeout; most defenses fail without encryption.

**Session hijacking succeeds when the session ID is predictable, exposed, reusable, or valid for too long.**

<table>
<colgroup>
<col style="width: 31%" />
<col style="width: 34%" />
<col style="width: 34%" />
</colgroup>
<thead>
<tr class="header">
<th></th>
<th><blockquote>
<p><strong>Web Session Hijacking</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>TCP Session Hijacking</strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>Target</p>
</blockquote></td>
<td><blockquote>
<p>Web authentication session</p>
</blockquote></td>
<td><blockquote>
<p>TCP connection</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Main thing stolen/used</p>
</blockquote></td>
<td><blockquote>
<p>Cookie/session token</p>
</blockquote></td>
<td><blockquote>
<p>TCP SEQ/ACK state</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>Layer</p>
</blockquote></td>
<td><blockquote>
<p>Application layer</p>
</blockquote></td>
<td><blockquote>
<p>Transport layer</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Phase 1</p>
</blockquote></td>
<td><blockquote>
<p>Find/track session</p>
</blockquote></td>
<td><blockquote>
<p>Track TCP connection</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>Phase 2</p>
</blockquote></td>
<td><blockquote>
<p>Obtain session token</p>
</blockquote></td>
<td><blockquote>
<p>Desynchronize</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Phase 3</p>
</blockquote></td>
<td><blockquote>
<p>Replay/use token</p>
</blockquote></td>
<td><blockquote>
<p>Inject packets</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>Memory</p>
</blockquote></td>
<td><blockquote>
<p>FIND → STEAL → REUSE</p>
</blockquote></td>
<td><blockquote>
<p>TRACK → DESYNC → INJECT</p>
</blockquote></td>
</tr>
</tbody>
</table>

# **4) How it works — the 3 phases (TCP Session Hijacking)**

| **Phase**                | **What the attacker does**                                                             |
|--------------------------|----------------------------------------------------------------------------------------|
| 1\. Track the connection | Use a sniffer to watch the victim; use Nmap to find predictable TCP sequence numbers   |
| 2\. Desynchronize        | Change the SEQ/ACK numbers so victim & server fall out of sync (send null data or RST) |
| 3\. Inject packets       | Insert the attacker's packets / take over as man-in-the-middle                         |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam anchor</strong></p>
<p><strong>•</strong> Track → Desynchronize → Inject.</p>
<p><strong>•</strong> To inject at network level, the attacker must know the Next Sequence Number (NSN).</p></td>
</tr>
</tbody>
</table>

**An attacker can inject packets without being a full MITM because they only need a way to get a suitably crafted packet to an endpoint; they don't necessarily need to relay all traffic between the two endpoints.**

# **5) Two big families (the \#1 exam split)**

| **Family**        | **Layer**      | **Attacks it works on**                                 |
|-------------------|----------------|---------------------------------------------------------|
| Application-level | Layer 7 (HTTP) | Steals/predicts the SESSION ID / cookie / token         |
| Network-level     | TCP/IP         | Hijacks the TCP session via sequence numbers & spoofing |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Passive vs Active (both families)</strong></p>
<p><strong>•</strong> Passive hijacking = only observe/record traffic (capture IDs &amp; passwords).</p>
<p><strong>•</strong> Active hijacking = take over the session (break the connection or act as MITM).</p></td>
</tr>
</tbody>
</table>

**Session Hijacking = Passive (observe/capture) → Active (use/manipulate) → Take over an existing session.**

# **6) Application-level hijacking (high yield)**

## **Ways to GET the session ID**

- Stealing — grab the ID (sniffing, XSS, trojan).

- Guessing — predict it by watching session variables.

- Brute forcing — try many possible IDs.

## **Key client-side attacks — memory map**

| **Group** | **Attack**           | **Core idea**                                            | **Exam keyword**            |
|-----------|----------------------|----------------------------------------------------------|-----------------------------|
| CODE      | XSS                  | Inject JS that reads document.cookie                     | Client-side code execution  |
| CODE      | Malicious JavaScript | JS captures & forwards the token                         | Token exfiltration          |
| TRUST     | CSRF                 | Victim's browser auto-sends cookies on a crafted request | One-click / session riding  |
| TRUST     | Session donation     | Attacker logs in, tricks victim into using HIS session   | Session misbinding          |
| REUSE     | Session replay       | Sniff a token, then replay the request                   | Replay authentication       |
| REUSE     | Session fixation     | Attacker fixes the session ID BEFORE login               | Pre-auth session ID         |
| NETWORK   | Proxy-based          | Victim routed through attacker's proxy                   | Man-in-the-Browser          |
| NETWORK   | CRIME                | Leak cookie via TLS/HTTP compression                     | Compression side-channel    |
| CRYPTO    | Forbidden / FREAK    | MITM abuses nonce reuse / weak TLS (AES-GCM)             | TLS downgrade / nonce reuse |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Mnemonic from your notes</strong></p>
<p><strong>•</strong> CODE → XSS, Malicious JS · TRUST → CSRF, Donation · REUSE → Replay, Fixation · NETWORK → Proxy, CRIME · CRYPTO → FREAK/Forbidden.</p></td>
</tr>
</tbody>
</table>

| **Category**   | **Technique**                 | **What happens**                                                                            | **Remember**             |
|----------------|-------------------------------|---------------------------------------------------------------------------------------------|--------------------------|
| Get Session ID | Stealing                      | Attacker obtains a valid session ID/token through sniffing, XSS, malware, etc.              | STEAL                    |
| Get Session ID | Guessing                      | Attacker predicts a weak/predictable session ID                                             | GUESS                    |
| Get Session ID | Brute Force                   | Attacker tries many possible session IDs                                                    | TRY MANY                 |
| CODE           | XSS                           | Malicious script runs in the victim's browser and may access available session data         | INJECT JS                |
| CODE           | Malicious JavaScript          | Script captures accessible information and sends it to the attacker                         | CAPTURE → EXFILTRATE     |
| TRUST          | CSRF                          | Victim's authenticated browser sends an attacker's crafted request with its cookies         | TRICK BROWSER            |
| TRUST          | Session Donation              | Attacker gets victim to use the attacker's authenticated session                            | GIVE MY SESSION          |
| REUSE          | Session Replay                | Attacker captures a valid session/token and uses it again                                   | CAPTURE → REUSE          |
| REUSE          | Session Fixation              | Attacker fixes/knows the session ID before the victim logs in                               | FIX ID → LOGIN           |
| NETWORK        | Proxy-based                   | Victim's web traffic passes through an attacker's proxy                                     | TRAFFIC THROUGH ATTACKER |
| BROWSER        | **Man-in-the-Browser (MitB)** | Malware in/around the browser modifies transactions while hiding the change from the victim | COMPROMISED BROWSER      |
| SIDE-CHANNEL   | CRIME                         | Compression side channel can reveal secrets such as session information                     | COMPRESSION → LEAK       |
| TLS            | FREAK                         | Historical TLS downgrade attack involving weak export-grade RSA                             | DOWNGRADE → WEAK RSA     |

## 

## **Man-in-the-Browser (MitB)**

- A trojan infects the browser and registers a handler that fires on target sites.

- It silently changes transaction values (e.g., payment amount/recipient), but shows the victim the ORIGINAL details on the receipt. The user notices nothing.

# **7) Network-level hijacking (high yield)**

| **Attack**                   | **Core idea**                                                       | **Exam keyword**             |
|------------------------------|---------------------------------------------------------------------|------------------------------|
| TCP/IP hijacking             | Predict TCP sequence numbers, spoof packets, desync victim & server | Sequence prediction / desync |
| IP spoofing (source routing) | Impersonate a trusted host; inject source-routed packets            | Trusted host abuse           |
| RST hijacking                | Send spoofed RST with a valid ACK to force a reset                  | TCP RST / forced reset       |
| Blind hijacking              | Inject data without seeing the responses                            | Blind injection, no sniffing |
| UDP hijacking                | Race/forge a UDP reply before the real server                       | Stateless / connectionless   |
| MITM via forged ICMP         | Fake ICMP errors reroute traffic through attacker                   | ICMP redirect                |
| ARP spoofing                 | Forged ARP replies poison the cache → traffic to attacker           | ARP poisoning (LAN)          |
| PetitPotam                   | Coerce a DC to authenticate (MS-EFSRPC), relay NTLM to AD CS        | NTLM relay / auth coercion   |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory map (network)</strong></p>
<p><strong>•</strong> TCP Sequence Abuse → TCP/IP, Blind, RST hijacking.</p>
<p><strong>•</strong> Trust Abuse → IP Spoofing, PetitPotam.</p>
<p><strong>•</strong> Stateless Abuse → UDP hijacking. Routing Abuse → ICMP forgery. LAN Poisoning → ARP spoofing.</p></td>
</tr>
</tbody>
</table>

**Application-level hijacking: Use it when the attacker wants to take over a specific application/web session and gain access to the user's account/session — “I want your SESSION.”**

**Network-level hijacking: Use it when the attacker wants to manipulate the communication between two machines by injecting, redirecting, intercepting, or disrupting network traffic — “I want your CONNECTION.”**

# **8) Ports / protocols to remember**

| **Item**            | **Role in hijacking**                                  |
|---------------------|--------------------------------------------------------|
| TCP 3-way handshake | Network-level hijacking abuses SEQ/ACK numbers here    |
| HTTP 80 / HTTPS 443 | App-level hijacking targets the cookie/session ID      |
| UDP (stateless)     | UDP hijacking forges replies (no handshake)            |
| ICMP                | Forged ICMP redirects enable MITM                      |
| TLS/SSL             | CRIME & Forbidden/FREAK attack weak crypto/compression |

# **9) Tools**

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 28%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><h1 id="tool"><strong>Tool</strong></h1></th>
<th><h1 id="purpose"><strong>Purpose</strong></h1></th>
<th><h1 id="free"><strong>Free?</strong></h1></th>
<th><h1 id="when-attacker-uses-it"><strong>When attacker uses it</strong></h1></th>
<th><h1 id="why-exam-takeaway"><strong>Why? / Exam takeaway</strong></h1></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><h1 id="hetty">Hetty</h1></td>
<td><h1 id="mitm-proxy-interceptreplay-http-requests">MITM proxy; intercept/replay HTTP requests</h1></td>
<td><h1 id="free-open-source">✅ Free / open source</h1></td>
<td><h1 id="when-testing-web-applications">When testing web applications</h1></td>
<td><h1 id="intercept-replay-web-traffic">Intercept &amp; replay web traffic</h1></td>
</tr>
<tr class="even">
<td><h1 id="caido">Caido</h1></td>
<td><h1 id="web-security-auditing-toolkit-proxy">Web security auditing toolkit / proxy</h1></td>
<td><h1 id="free-tier-available-some-featuresplans-may-be-paid">⚠️ Free tier available; some features/plans may be paid</h1></td>
<td><h1 id="when-testing-web-applications-1">When testing web applications</h1></td>
<td><h1 id="modern-web-proxy-auditing-tool">Modern web proxy / auditing tool</h1></td>
</tr>
<tr class="odd">
<td><h1 id="bettercap">Bettercap</h1></td>
<td><h1 id="mitm-and-network-attacks">MITM and network attacks</h1></td>
<td><h1 id="free-open-source-1">✅ Free / open source</h1></td>
<td><h1 id="when-attackingtesting-network-traffic-especially-lan">When attacking/testing network traffic, especially LAN</h1></td>
<td><h1 id="network-mitm-traffic-manipulation">Network MITM / traffic manipulation</h1></td>
</tr>
<tr class="even">
<td><h1 id="colasoft-packet-builder">Colasoft Packet Builder</h1></td>
<td><h1 id="createcraft-custom-packets">Create/craft custom packets</h1></td>
<td><h1 id="commercial-trialfree-limitations-may-vary">⚠️ Commercial; trial/free limitations may vary</h1></td>
<td><h1 id="when-creating-specific-forged-packets-such-as-tcp-rst">When creating specific forged packets, such as TCP RST</h1></td>
<td><h1 id="craft-spoofed-packets">Craft spoofed packets</h1></td>
</tr>
<tr class="odd">
<td><h1 id="wireshark">Wireshark</h1></td>
<td><h1 id="capture-and-analyze-network-traffic">Capture and analyze network traffic</h1></td>
<td><h1 id="free-open-source-2">✅ Free / open source</h1></td>
<td><h1 id="when-observinganalyzing-traffic-or-looking-for-signs-of-hijacking">When observing/analyzing traffic or looking for signs of hijacking</h1></td>
<td><h1 id="packet-analysis-detection">Packet analysis / detection</h1></td>
</tr>
<tr class="even">
<td><h1 id="usm-anywhere">USM Anywhere</h1></td>
<td><h1 id="siemsecurity-monitoring-platform">SIEM/security monitoring platform</h1></td>
<td><h1 id="commercial">❌ Commercial</h1></td>
<td><h1 id="when-an-organization-wants-centralized-security-monitoringdetection">When an organization wants centralized security monitoring/detection</h1></td>
<td><h1 id="siem-detection">SIEM / detection</h1></td>
</tr>
</tbody>
</table>

# **10) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> On a shared Wi-Fi, an attacker sniffs the victim's unencrypted HTTP traffic (link to Module 08).</p>
<p><strong>•</strong> He captures the session cookie (the 'wristband') from the HTTP header.</p>
<p><strong>•</strong> He loads the cookie into his browser and is now logged in as the victim — no password needed.</p>
<p><strong>•</strong> This is application-level hijacking via session sniffing.</p>
<p><strong>•</strong> Fix: HTTPS everywhere, HttpOnly + Secure + SameSite cookies, and regenerate the session ID after login.</p></td>
</tr>
</tbody>
</table>

# **11) Detection & countermeasures**

## **Prevent session hijacking**

- Use HTTPS + HSTS (HTTP Strict Transport Security) everywhere.

- Regenerate the session ID AFTER login (kills fixation).

- Strong random session IDs; short timeouts; invalidate on logout.

- Cookie flags: HttpOnly (blocks JS theft), Secure (HTTPS only), SameSite (blocks CSRF).

- Token Binding; anti-CSRF tokens.

## **Prevent MITM (feeds hijacking)**

- DNS over HTTPS (DoH), WPA3, VPN, 2FA, PKI, Zero-Trust, network segmentation.

- **IPsec modes:** Transport = encrypts only the payload; Tunnel = encrypts the ENTIRE IP packet.

# **12) Related concepts**

| **Concept**                | **One-line meaning**                                                                  |
|----------------------------|---------------------------------------------------------------------------------------|
| Session ID / token         | The 'wristband' **'bracelet'** that proves you're logged in |
| Spoofing vs Hijacking      | New session w/ stolen creds vs seizing a live session                                 |
| Desynchronization          | Making victim & server disagree on SEQ numbers                                        |
| Next Sequence Number (NSN) | The number the attacker must predict to inject                                        |
| Man-in-the-Browser         | Trojan inside the browser altering transactions live                                  |

# **13) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Spoofing = NEW session (stolen creds). Hijacking = EXISTING active session. Don't swap.</p>
<p><strong>•</strong> Session fixation sets the ID BEFORE login; defense = regenerate ID AFTER login.</p>
<p><strong>•</strong> XSS steals cookies via JS → defense is HttpOnly. CSRF rides an existing session → defense is anti-CSRF tokens + SameSite.</p>
<p><strong>•</strong> Blind hijacking = inject WITHOUT seeing replies (must predict SEQ).</p>
<p><strong>•</strong> RST hijacking RESETS/ends the session; it doesn't read data.</p>
<p><strong>•</strong> Network-level = TCP sequence numbers; Application-level = session ID/cookie.</p>
<p><strong>•</strong> Hijacking needs the user to connect FIRST (an active session must exist).</p></td>
</tr>
</tbody>
</table>

# **14) Mnemonics**

- **Phases:** 'Track, Desync, Inject' → TDI.

- **App-level groups:** CODE · TRUST · REUSE · NETWORK · CRYPTO.

- **Spoof vs Hijack:** 'Spoof starts fresh, Hijack steals live.'

- **Cookie flags:** 'HSS' → HttpOnly (no JS), Secure (HTTPS), SameSite (no CSRF).

- **Fixation fix:** 'New login = New ID.'

# **15) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Session hijacking = take over a live session by stealing/predicting the session ID. Auth happens once.</p>
<p><strong>•</strong> Spoofing = new session (stolen creds); Hijacking = existing active session.</p>
<p><strong>•</strong> Phases: Track → Desynchronize → Inject. Inject needs the Next Sequence Number.</p>
<p><strong>•</strong> 2 families: Application-level (session ID/cookie) vs Network-level (TCP sequence).</p>
<p><strong>•</strong> App attacks: XSS (HttpOnly), CSRF (SameSite + anti-CSRF token), replay, fixation (regenerate ID), CRIME, donation, MitB.</p>
<p><strong>•</strong> Network attacks: TCP/IP hijack, RST hijack, blind hijack, IP spoof, UDP hijack, ICMP MITM, ARP spoof, PetitPotam.</p>
<p><strong>•</strong> Passive = observe/capture; Active = take over / MITM.</p>
<p><strong>•</strong> Tools: Hetty, bettercap, Caido, Colasoft (attack); Wireshark, USM (detect).</p>
<p><strong>•</strong> Defense: HTTPS+HSTS, regenerate session ID after login, HttpOnly/Secure/SameSite cookies, short timeouts, 2FA.</p>
<p><strong>•</strong> IPsec: Transport = payload only; Tunnel = whole packet.</p></td>
</tr>
</tbody>
</table>

*End of Module 11 — Session Hijacking. Next up: quiz this module, or move to Module 12 (Evading IDS, Firewalls & Honeypots).*
