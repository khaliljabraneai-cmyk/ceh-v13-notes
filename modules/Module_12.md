**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 12 — Evading IDS, Firewalls & Honeypots**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Google Drive (CEH V13 Master Study Guide) + Project documents (12 - Evading IDS, Firewalls and Honeypots.md). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

This module covers the three main network defenses — **IDS (detects),** **Firewall (blocks),** **Honeypot (traps)** — and the techniques attackers use to slip past or fool each one.

Traps : pièges

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>The guarded-building picture</strong></p>
<p><strong>•</strong> Firewall = the locked gate that decides who may enter.</p>
<p><strong>•</strong> IDS = the security camera / alarm that watches and reports suspicious activity (but doesn't stop it).</p>
<p><strong>•</strong> IPS = a guard that watches AND physically blocks the intruder.</p>
<p><strong>•</strong> Honeypot = a fake room full of 'treasure' to lure and study the thief.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **2) The three defenses (know the difference)**

| **Device** | **Job**                         | **Key point**                                      |
|------------|---------------------------------|----------------------------------------------------|
| Firewall   | Allow or block traffic by rules | Gatekeeper at the network boundary                 |
| IDS        | DETECT & alert on intrusions    | Passive — raises an alarm, doesn't stop it         |
| IPS        | DETECT & PREVENT                | Active IDS — blocks malicious traffic in real time |
| Honeypot   | Lure & study attackers          | Decoy with no real production value                |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Exam anchor</strong></p>
<p><strong>•</strong> IDS = detect only (alarm). IPS = detect + block (active). This single difference is tested constantly.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **3) How an IDS detects intrusions**

| **Method**                     | **How it works**                               | **Weakness**                |
|--------------------------------|------------------------------------------------|-----------------------------|
| Signature recognition (misuse) | Matches traffic to known attack signatures     | Misses NEW/zero-day attacks |
| Anomaly detection              | Flags deviations from normal baseline behavior | More false positives        |
| Protocol anomaly               | Flags deviations from protocol standards       | Needs good protocol models  |

| **Method**       | **How IDS detects**                                    | **What it needs**                                      | **Main weakness**                       |
|------------------|--------------------------------------------------------|--------------------------------------------------------|-----------------------------------------|
| Signature-based  | Matches traffic against known attack patterns          | Updated signatures/rules from vendor/security provider | ❌ May miss new/zero-day attacks        |
| Anomaly-based    | Detects behavior that deviates from normal behavior    | Normal baseline of the network/users                   | ❌ Can generate false positives         |
| Protocol anomaly | Checks whether traffic follows expected protocol rules | Protocol specifications/models                         | ❌ Needs accurate protocol models       |
| Combination      | Uses several methods together                          | Signatures + baselines + protocol analysis             | ✅ Better coverage, but more complexity |

NIST notes that modern network IDPS products **<u>commonly combine</u>** signature, anomaly, and stateful protocol-analysis techniques

## **IDS alert types (memorize the 2x2)**

| Alert          | Situation            | Meaning                            | Simple example                                  |
|----------------|----------------------|------------------------------------|-------------------------------------------------|
| True Positive  | Attack → Alert       | Correct alarm on a **real attack** | Hacker attacks server → IDS alerts 🚨           |
| False Positive | No attack → Alert    | False alarm (noise)                | Employee downloads a large file → IDS alerts 🚨 |
| False Negative | Attack → No alert    | Missed attack (**most dangerous**) | Hacker attacks server → IDS stays silent 🤫     |
| True Negative  | No attack → No alert | Correctly silent                   | Employee normally browses → IDS stays silent 🤫 |

## **IDS placement & types**

 WHERE an IDS can be placed and WHAT it monitors.

| **Type** | **Where?**             | **What does it watch?**                | **Easy memory**             |
|----------|------------------------|----------------------------------------|-----------------------------|
| **NIDS** | On the **network**     | Network traffic / packets              | 👀 **Watches the network**  |
| **HIDS** | On a **host/computer** | Files, logs, processes, system changes | 👀 **Watches one computer** |

- **NIDS (network):** a 'black box' in promiscuous mode watching network traffic (e.g., detects DDoS).

- **HIDS (host):** installed on one host; detects file changes etc.; resource-heavy.

- **Placement:** best practice = one IDS OUTSIDE the firewall and one INSIDE near the DMZ.

**DMZ (Demilitarized Zone)** = a separate network area between the Internet and the internal network./ Public servers, isolated from the internal network.

<img src="media/image1.png" style="width:7in;height:3.88125in" />

# **4) Firewall types (by working mechanism)**

| **Firewall type**         | **OSI layer** | **How it works**                             |
|---------------------------|---------------|----------------------------------------------|
| Packet-filtering          | L3 / L4       | Filters by IP, port, protocol (stateless)    |
| Circuit-level gateway     | L5 (session)  | Validates the TCP 3-way handshake            |
| Application-level (proxy) | L7            | Inspects application data; proxy-based       |
| Stateful multilayer       | L3–L7         | Tracks connection state (e.g., Cisco PIX)    |
| NGFW (next-gen)           | L3–L7         | Deep packet inspection + app awareness + IPS |

| **Firewall type**           | **OSI layer** | **Simple idea**           | **What it checks**                  | **How the guard behaves**                                                                   | **Weakness**                                              | **When to choose it**                                                                                                                       | **Market examples**                                                                          |
|-----------------------------|---------------|---------------------------|-------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **Packet-filtering**        | L3 / L4       | 🚪 Basic filter           | IP + port + protocol                | Reads only the envelope, and judges each packet alone with no memory (stateless)            | Fooled by forged addresses; never looks inside            | A simple rule on a router where speed matters most, such as blocking a list of unwanted IP addresses at the network edge                    | Cisco router access lists (ACLs), Linux nftables / iptables, AWS network ACLs                |
| **Circuit-level gateway**   | L5 (session)  | 🤝 Checks the connection  | TCP session / 3-way handshake       | Confirms both sides greeted each other properly before the conversation starts              | Does not look at what is said afterwards                  | Hiding internal computers behind one gateway at low cost, such as a SOCKS proxy that staff connections pass through                         | SOCKS5 proxy servers such as Dante                                                           |
| **Application-level proxy** | L7            | 🔍 Checks the application | Application data (HTTP, FTP, etc.)  | Acts as a middleman: takes the message, reads it fully, and delivers it himself             | Slow; must understand each application                    | Protecting one specific service that needs full content checks, such as a web proxy filtering staff browsing or a WAF in front of a website | F5 BIG-IP, Cloudflare WAF, Fortinet FortiWeb, Squid (free web proxy)                         |
| **Stateful multilayer**     | L3–L7         | 🧠 Remembers connections  | Connection state + multiple layers  | Keeps a logbook of ongoing conversations, so he knows a real reply from an uninvited packet | Only a light understanding of what applications are doing | Separating internal zones on a limited budget, such as a firewall between the staff network and the server room                             | Cisco ASA (successor of PIX), Juniper SRX, pfSense and OPNsense (free)                       |
| **NGFW**                    | L3–L7         | 🧠🔍 Advanced firewall    | Apps + deep packet inspection + IPS | Does all of the above, recognises the application itself, and blocks known attacks          | Expensive; needs tuning and regular updates               | The main internet entrance of a company, where you want firewall, application control, and IPS in one device                                | Fortinet FortiGate, Palo Alto Networks PA-Series, Check Point Quantum, Cisco Secure Firewall |

## **Firewall architecture terms**

- **Bastion host:** hardened host facing the internet; mediator between inside and outside.

- **DMZ (screened subnet):** neutral buffer zone between the internal network and the untrusted internet.

- **Multi-homed firewall:** one device with several NICs connecting separate segments.

| **Concept**             | **What is it?**                                                                         | **Main purpose**                                            | **Easy example**                         | **🧠 Remember**         |
|-------------------------|-----------------------------------------------------------------------------------------|-------------------------------------------------------------|------------------------------------------|-------------------------|
| Bastion Host 🛡️         | A specific computer/server that is strongly hardened and exposed to an external network | Provide a secure, controlled point for a service or access  | Hardened Web Server                      | Bastion = hardened HOST |
| DMZ 🚧                  | A separate network zone between the Internet and the internal network                   | Isolate public-facing servers from the internal network     | Zone containing Web/Mail/DNS servers     | DMZ = network ZONE      |
| Multi-homed Firewall 🔌 | A firewall with multiple network interfaces (NICs)                                      | Connect and control traffic between different network zones | One firewall → Internet + DMZ + Internal | Multi-homed = many NICs |

# 

# **5) Step 1 — Identify the defense**

This section is basically asking you to identify what information an attacker can discover using each reconnaissance technique.

| **Technique**   | **What it finds**                                           | **Very simple idea**                |
|-----------------|-------------------------------------------------------------|-------------------------------------|
| Port scanning   | Open ports / exposed services                               | “What doors are open?”              |
| Firewalking     | Firewall ACLs / which traffic can pass through the firewall | “What can pass the firewall?”       |
| Banner grabbing | Service software and versions                               | “What software is behind the door?” |

# **6) Firewall / IDS evasion techniques (high yield)**

<table>
<colgroup>
<col style="width: 21%" />
<col style="width: 49%" />
<col style="width: 28%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Technique</strong></th>
<th><strong>How it evades</strong></th>
<th><strong>Key clue / tool</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>IP address spoofing</td>
<td>Forge (<mark>Falsifier</mark>) the source IP</td>
<td>Hping</td>
</tr>
<tr class="even">
<td>Source routing</td>
<td>Force packets through less-monitored paths</td>
<td>Alternate route</td>
</tr>
<tr class="odd">
<td>Tiny fragments</td>
<td>Split TCP header across fragments to hide it</td>
<td>Fragmentation</td>
</tr>
<tr class="even">
<td>Proxy server</td>
<td>Route through a proxy to hide source</td>
<td>Hide identity</td>
</tr>
<tr class="odd">
<td>ICMP tunneling</td>
<td>Hide payload in ICMP echo data</td>
<td>ICMPTX</td>
</tr>
<tr class="even">
<td>ACK tunneling</td>
<td><p>Firewalls often trust ACK packets of 'established' sessions</p>
<p>ACK = looks like it belongs to an already-established connection</p></td>
<td>Hping ACK</td>
</tr>
<tr class="odd">
<td>HTTP tunneling</td>
<td><p>Wrap traffic in HTTP over port 80</p>
<p><strong>HTTP tunneling</strong> means <strong>hiding/encapsulating other traffic inside HTTP traffic</strong>. Make my traffic look like normal web traffic</p></td>
<td>Chisel</td>
</tr>
<tr class="even">
<td>SSH tunneling</td>
<td><p>Encrypt &amp; forward traffic via SSH</p>
<p><strong>SSH tunneling</strong> means using an <strong>SSH connection as a secure tunnel to carry other traffic</strong>. “Put my traffic inside an encrypted SSH tunnel.”</p></td>
<td>OpenSSH, Bitvise</td>
</tr>
<tr class="odd">
<td>DNS tunneling</td>
<td><p>Hide data in DNS queries; DNSSEC can't detect</p>
<p><strong>DNS tunneling</strong> means <strong>hiding data inside DNS queries and responses</strong>. “Use DNS as a tunnel to carry hidden data.”</p></td>
<td>iodine, dnscat2</td>
</tr>
</tbody>
</table>

# **7) Classic IDS evasion techniques (exam favorites)**

| **Technique**                    | **Core idea**                                                                                    |
|----------------------------------|--------------------------------------------------------------------------------------------------|
| Insertion attack                 | Make the IDS accept packets the host will REJECT (IDS sees a different stream)                   |
| Evasion                          | IDS REJECTS packets the host ACCEPTS (opposite of insertion)                                     |
| Session splicing                 | Split the attack across MANY tiny packets so no single packet matches a signature (tool: Nessus) |
| Fragmentation                    | Exceed MTU so packets split; IDS may fail to reassemble                                          |
| Unicode evasion                  | Encode characters many ways (e.g., %u2215 for '/') to dodge signatures                           |
| Obfuscating / encoding           | Encode payload so only the target can decode it, not the IDS                                     |
| Polymorphic shellcode            | Change the shellcode signature each time                                                         |
| DoS against IDS                  | Overload the IDS so it stops inspecting                                                          |
| False positive flooding          | Trigger many fake alerts to hide the real attack in the noise                                    |
| TTL manipulation                 | Craft TTLs so a packet dies before the host but after the IDS (needs topology knowledge)         |
| Urgency flag / invalid RST       | Abuse TCP URG pointer or bad-checksum RST that IDS handles differently from host                 |
| Desynchronization (pre/post SYN) | Send SYNs with bad checksums / odd sequence to confuse IDS state                                 |

| **Technique**                    | **Core idea — very simple**                                                                     |
|----------------------------------|-------------------------------------------------------------------------------------------------|
| Insertion attack                 | Send something the IDS accepts, but the victim rejects → confuse the IDS.                       |
| Evasion                          | Send something the IDS misses/rejects, but the victim accepts → bypass IDS detection.           |
| Session splicing                 | Break the attack into many small pieces so the IDS may not recognize the complete attack.       |
| Fragmentation                    | Split one packet into smaller fragments so the IDS may have difficulty inspecting it correctly. |
| Unicode evasion                  | Represent characters in a different form so the IDS may not recognize the attack.               |
| Obfuscating / encoding           | Change the appearance of the payload to hide the malicious content from the IDS.                |
| Polymorphic shellcode            | Change the shellcode's signature/appearance each time while keeping the same function.          |
| DoS against IDS                  | Send too much traffic to overload the IDS and reduce its ability to inspect traffic.            |
| False positive flooding          | Create many fake alerts so the real attack is hidden in the noise.                              |
| TTL manipulation                 | Manipulate how far a packet travels, causing IDS and victim to see different behavior.          |
| Urgency flag / invalid RST       | Send unusual TCP packets that IDS and victim may interpret differently → confuse IDS.           |
| Desynchronization (pre/post SYN) | Make the IDS lose track of the TCP session, so it misunderstands later traffic.                 |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Insertion vs Evasion (don't swap)</strong></p>
<p><strong>•</strong> Insertion = IDS ACCEPTS what the host REJECTS (IDS over-reads).</p>
<p><strong>•</strong> Evasion = IDS REJECTS what the host ACCEPTS (IDS under-reads → attack slips to host).</p>
<p><strong>•</strong> Both work by making the IDS and the host see DIFFERENT data.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **8) WAF, NAC & endpoint-security evasion (v13)**

## **Bypassing a WAF (Web App Firewall)**

- **WAF = Web Application Firewall**

> It protects a **web application** and examines HTTP/HTTPS requests.

| **Technique**                 | **Very simple meaning**                                                             | **Memory**                  |
|-------------------------------|-------------------------------------------------------------------------------------|-----------------------------|
| **HTTP header spoofing**      | Modify/fake HTTP headers to fool the WAF                                            | **Fake headers**            |
| **Blacklist keyword evasion** | Change malicious words so they don't match the blacklist                            | **Hide the keyword**        |
| **Fuzzing / brute forcing**   | Try many variations until one gets through                                          | **Try many versions**       |
| **Weak SSL/TLS ciphers**      | Exploit weak encryption/configuration                                               | **Exploit weak crypto**     |
| **HTML smuggling**            | Use HTML/JavaScript to construct/deliver a payload in a way that can bypass filters | **Build payload on client** |

- **WAF bypass = Make the web request look acceptable.**

## **NAC bypass**

**NAC = Network Access Control.** NAC controls which users/devices are allowed to connect to the network.

- **VLAN hopping:** abuse DTP to switch a port to trunk (tool: VLANPWN). The attacker tries to move from one VLAN to another

- **Pre-authenticated device:** hide a Raspberry Pi behind an authorized device (FENRIR).

## **Endpoint / AV evasion**

**Now we're no longer talking about the network. We're talking about the computer itself.**

| **Technique**                 | **Very simple meaning**                                    | **Memory**                 |
|-------------------------------|------------------------------------------------------------|----------------------------|
| **Ghostwriting**              | Change/rewrite malware so it looks different               | **Change malware**         |
| **DLL hijacking**             | Trick a legitimate program into loading a malicious DLL    | **Hijack the DLL**         |
| **Process injection**         | Put malicious code inside another legitimate process       | **Hide inside a process**  |
| **LoLBins**                   | Use legitimate system tools for malicious purposes         | **Use trusted tools**      |
| **AMSI PowerShell downgrade** | Use an older PowerShell mode to avoid some AMSI inspection | **Downgrade PowerShell**   |
| **Fast-flux DNS**             | Frequently change IP addresses associated with a domain    | **Change IPs quickly**     |
| **Timing/sleep evasion**      | Wait before executing malicious behavior                   | **Wait to evade analysis** |

**INTERNET**

**│**

**\[ FIREWALL \]**

**Controls traffic**

**│**

**Company Network**

**│**

**┌──────────┴──────────┐**

**│ │**

**\[NAC\] \[WAF\]**

**Who can join? Protects website**

**│**

**\[Laptop\]**

**│**

**\[Endpoint Security\]**

**Protects the laptop**

# **9) Honeypots**

A honeypot is a decoy system with no real value — any interaction with it is suspicious, so it logs attacks and gives early warning.

A honeypot is mainly used by **security teams** to **attract and detect attackers**.

| **Security control** | **Main job**                       | **Simple question**                             |
|----------------------|------------------------------------|-------------------------------------------------|
| 🛡️ **Firewall**      | Control network traffic            | **“Should this traffic pass?”**                 |
| 🔍 **IDS**           | Detect attacks                     | **“Is this suspicious?”**                       |
| 🚫 **IPS**           | Detect + block                     | **“Is this dangerous? Block it!”**              |
| 🌐 **WAF**           | Protect web applications           | **“Is this web request malicious?”**            |
| 🔑 **NAC**           | Control network entry              | **“Can this device/user join?”**                |
| 🍯 **Honeypot**      | Attract + detect + study attackers | **“Who is attacking and what are they doing?”** |

**How much can the attacker interact with the fake system?**

| **Type**               | **What it emulates**                                     |
|------------------------|----------------------------------------------------------|
| Low-interaction        | A few services/apps only (KFSensor, honeytrap)           |
| Medium-interaction     | A simulated OS + some apps (HoneyBOT)                    |
| High-interaction       | Nearly all services of a real target (most risk/insight) |
| Pure honeypot          | Emulates a full real production network                  |
| Production vs Research | In-house, limited data vs run by researchers for study   |
| Honeynet               | A whole network of honeypots                             |

## **Detecting & defeating honeypots (attacker side)**

- This section changes perspective. Before, we studied how defenders use honeypots. Now we're looking at how an attacker might recognize that a target is a honeypot.

<!-- -->

- **Fingerprint** services (nmap -sV), analyze response latency, check MAC/OUI irregularities, spot unexpected open ports. **Look for signs that the target is fake.**

- **Tar pits** (Layer 2/4/7) slow attackers. **A tar pit is designed to slow down attackers.**

- **Bait-and-switch** redirects attackers to a honeypot. Bait-and-switch = **Redirect attacker → honeypot. the defender secretly redirects the attacker**

- Detection tool: **Send-Safe Honeypot Hunter Tool used to help identify whether a target may be a honeypot.**

# **10) Ports, protocols & tools**

This section is basically a CEH revision table: know what the protocol/port is used for and what each tool does.

| **Item**          | **Role in this module**                        | **🧠 Memory**                   |
|-------------------|------------------------------------------------|---------------------------------|
| **TCP 80 — HTTP** | HTTP tunneling (e.g. Chisel)                   | **Hide traffic in HTTP**        |
| **TCP 22 — SSH**  | SSH tunneling / port forwarding                | **Tunnel through SSH**          |
| **UDP 53 — DNS**  | DNS tunneling (iodine, dnscat2)                | **Hide data in DNS**            |
| **ICMP**          | ICMP tunneling; ACK tunneling uses **TCP ACK** | **Hide data in ICMP / use ACK** |

| **Tool** | **Purpose**                       | **🧠 Memory** | **Free?** | **Open Source?** |
|------------------------------------|-------------------------------------------------------------|-----------------------------------------|-------------------------------------|--------------------------------------------|
| **Snort**                          | IDS/IPS; detects suspicious **network traffic** using rules | **Detect network attacks**              | ✅ Yes                              | ✅ Yes                                     |
| **Suricata**                       | IDS/IPS; detects suspicious **network traffic**             | **Detect network attacks**              | ✅ Yes                              | ✅ Yes                                     |
| **YARA**                           | Identifies/classifies **malware** using rules and patterns  | **Find malware patterns**               | ✅ Yes                              | ✅ Yes                                     |
| **yarGen**                         | Generates YARA rules for malware samples                    | **Generate malware rules**              | ✅ Yes                              | ✅ Yes                                     |
| **Hping**                          | Creates/sends customized packets                            | **Build packets**                       | ✅ Yes                              | ✅ Yes                                     |
| **Colasoft Packet Builder**        | Creates customized network packets                          | **Build packets**                       | ⚠️ Free version                     | ❌ No                                      |
| **Nessus**                         | Scans systems for **vulnerabilities**                       | **Find vulnerabilities**                | ⚠️ Limited free version             | ❌ No                                      |
| **Traffic IQ Professional**        | Traffic/security analysis and evasion testing               | **Test evasion**                        | ❌ Paid                             | ❌ No                                      |
| **HoneyBOT**                       | Honeypot for detecting suspicious/attacker activity         | **Detect attackers**                    | ⚠️ Free                             | ❌ No                                      |
| **Blumira**                        | Security monitoring / detection platform                    | **Monitor security**                    | ⚠️ Free options                     | ❌ No                                      |

# **11) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> An attacker wants to exfiltrate data but the firewall blocks unusual outbound ports.</p>
<p><strong>•</strong> He notices DNS (UDP 53) is always allowed outbound.</p>
<p><strong>•</strong> He uses dnscat2 to hide stolen data inside DNS queries (DNS tunneling).</p>
<p><strong>•</strong> DNSSEC doesn't catch it, and the IDS treats it as normal lookups.</p>
<p><strong>•</strong> Defense: inspect/limit DNS query volume &amp; size, egress filtering, and DNS monitoring.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **12) Countermeasures**

- Keep IDS/IPS signatures and firewall rules updated; combine signature + anomaly detection.

- Normalize/reassemble traffic at the IDS (defeats fragmentation & session splicing).

- Strict egress filtering; inspect DNS/ICMP/HTTP for tunneling; disable source routing.

- Deep packet inspection (NGFW), TLS inspection, and DMZ segmentation.

- Deploy honeypots for early warning; monitor for scanning & false-positive floods.

# **13) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> IDS detects only; IPS detects AND blocks. Firewall allows/blocks by rules.</p>
<p><strong>•</strong> False NEGATIVE (missed attack) is the most dangerous alert type.</p>
<p><strong>•</strong> Insertion (IDS accepts what host rejects) vs Evasion (IDS rejects what host accepts).</p>
<p><strong>•</strong> Session splicing = many TINY packets to dodge signatures (tool: Nessus).</p>
<p><strong>•</strong> Firewalking uses TTL to map firewall rules; banner grabbing reads service info.</p>
<p><strong>•</strong> DNS tunneling hides data in DNS and DNSSEC can't detect it.</p>
<p><strong>•</strong> ACK tunneling works because firewalls trust ACK packets of 'established' sessions.</p>
<p><strong>•</strong> Honeypot = decoy; ANY interaction with it is suspicious by design.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **14) Mnemonics**

- **3 defenses:** 'Fire Detects, IPS Protects, Honey Attracts' → Firewall blocks, IDS detects, IPS prevents, Honeypot lures.

- **IDS alerts:** 'TP, FP, FN, TN' — False Negative = the dangerous miss.

- **Tunnels:** 'I Ate Happy Snacks Daily' → ICMP, ACK, HTTP, SSH, DNS tunneling.

- **Insertion vs Evasion:** 'Insertion = IDS eats extra; Evasion = IDS skips.'

- **Splicing:** 'Slice it small, no signature at all.'

# **15) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Firewall = block/allow; IDS = detect (alarm); IPS = detect + block; Honeypot = decoy/trap.</p>
<p><strong>•</strong> IDS detection: signature (misses zero-day), anomaly, protocol anomaly.</p>
<p><strong>•</strong> Alerts: TP, FP, FN (missed = worst), TN.</p>
<p><strong>•</strong> Firewalls: packet-filtering (L3/4 stateless), circuit (L5), proxy (L7), stateful (L3-7), NGFW (DPI+IPS).</p>
<p><strong>•</strong> Identify defense: port scan, firewalking (TTL), banner grabbing.</p>
<p><strong>•</strong> Evasion: IP spoofing (Hping), source routing, tiny fragments, proxy, tunneling (ICMP/ACK/HTTP/SSH/DNS).</p>
<p><strong>•</strong> IDS evasion: insertion, evasion, session splicing (Nessus), fragmentation, Unicode, polymorphic shellcode, false-positive flood, TTL tricks.</p>
<p><strong>•</strong> WAF bypass: header spoofing, HTML smuggling, cipher abuse. NAC: VLAN hopping (DTP), rogue Raspberry Pi.</p>
<p><strong>•</strong> AV evasion: ghostwriting, DLL hijacking, process injection, LoLBins, AMSI downgrade, fast-flux DNS.</p>
<p><strong>•</strong> Honeypots: low/medium/high interaction, honeynet. Detect via nmap fingerprinting, latency, MAC/OUI.</p>
<p><strong>•</strong> Tools: Snort/Suricata (IDS), YARA (yarGen), Hping/Colasoft (crafting), Nessus (splicing), HoneyBOT.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

*End of Module 12 — Evading IDS, Firewalls & Honeypots. Next up: quiz this module, or move to Module 13 (Hacking Web Servers).*
