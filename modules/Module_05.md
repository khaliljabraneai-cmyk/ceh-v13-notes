**Module 05 — Vulnerability Analysis**

If Enumeration (Module 04) was walking through the house cataloguing every drawer and cabinet, **Vulnerability Analysis is the home inspector checking which of those drawers has a broken lock, which cabinet hinge is rusted through, and which window frame is rotting** — systematically identifying *actual weaknesses* that could be exploited, not just what exists. This is **Phase 4 of the CEH Hacking Methodology** (Footprinting → Scanning → Enumeration → Vulnerability Analysis → System Hacking). Careful: this is the study/module methodology, NOT the 5 Phases of Hacking (Recon → Scanning → Gaining → Maintaining → Covering), which has no Vulnerability Analysis step at all.

**1. What is Vulnerability Analysis? 🏠**

Per your Master Study Guide, this module covers identifying, classifying, and prioritizing vulnerabilities. The 3 official learning objectives (confirmed via supplementary source) are:

1.  **Summarize Vulnerability Assessment Concepts**

2.  **Use Vulnerability Assessment Tools**

3.  **Analyse Vulnerability Assessment Reports**

**Goal:** Take the "what's alive/open" picture from Scanning + the "what details exist" picture from Enumeration, and answer: **"which of these things are actually exploitable weaknesses?"**

**2. Vulnerability vs. Threat vs. Risk (refresher from Module 01 — reapplied here)**

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 85%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Term</strong></th>
<th><strong>House Analogy (reapplied)</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Vulnerability</strong></td>
<td><p>"A weakness that an attacker can exploit to perform unauthorized actions within a computer or network system"</p>
<p><strong>Vulnerability = weakness.</strong></p></td>
</tr>
<tr class="even">
<td><strong>Threat</strong></td>
<td><p>"Something that has the possibility to incur a breach of confidentiality, integrity, or availability"</p>
<p><strong>Threat = something that can cause harm.</strong></p></td>
</tr>
<tr class="odd">
<td><strong>Risk</strong></td>
<td><p>"A specific level of uncertainty regarding a potential event that could cause damage to your organization's assets, availability, or reputation"</p>
<p><strong>Risk = the potential damage/loss resulting from a threat exploiting a vulnerability.</strong></p></td>
</tr>
</tbody>
</table>

**RISK = Threat × Vulnerability × Impact**

**Risk = the potential for damage when a threat exploits a vulnerability.**

This module is entirely about **finding and classifying the vulnerabilities** — the rusted hinges — before anyone decides to exploit them.

🔴 **Vulnerability = Weakness**  
🔴 **Threat = Danger**  
🔴 **Risk = Potential damage/loss**

**3. CVE, CVSS, and Vulnerability Databases (heavily tested)**

| **\#** | **Term/Site**                                  | **URL**          | **Description**                                                                                                                             | **Key Difference**                                                                                                            |
|--------|------------------------------------------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| 1      | **CVE (Common Vulnerabilities and Exposures)** | cve.org          | A standardized naming/catalog system that assigns a unique ID (e.g., CVE-2024-12345) to a publicly known vulnerability                      | Just the identity/name of the flaw — no severity, no scoring, no context on affected systems                                  |
| 2      | **NVD (National Vulnerability Database)**      | nvd.nist.gov     | NIST's database that takes each CVE and enriches it with CVSS score, CPE (affected products/versions), and CWE (weakness category)          | The enrichment layer — turns a bare CVE ID into actionable data (how bad, what's affected)                                    |
| 3      | **CVSS (Common Vulnerability Scoring System)** | first.org/cvss   | A scoring framework/methodology (0.0–10.0 scale) that calculates severity based on attack vector, complexity, privileges needed, and impact | Not a database or catalog — it's the scoring formula/standard that NVD (and others) apply to a CVE                            |
| 4      | **MITRE ATT&CK**                               | attack.mitre.org | A knowledge base of real-world adversary tactics, techniques, and procedures (TTPs) mapped to attack lifecycle stages                       | Doesn't track vulnerabilities at all — it tracks attacker behavior, i.e. how techniques get used regardless of a specific CVE |

**Easy memory trick:** CVE = the *name/ID card* of the vulnerability. CVSS = the *severity grade* on that ID card.

3b. CVSS Severity Ranges (memorize these numbers) 🎯

CVE tells you **which** bug it is; CVSS tells you **how bad** it is on a 0–10 scale. The exam loves asking you to map a score to a severity band:

| **Severity** | **CVSS base score** |
|--------------|---------------------|
| None         | 0.0                 |
| Low          | 0.1 – 3.9           |
| Medium       | 4.0 – 6.9           |
| High         | 7.0 – 8.9           |
| Critical     | 9.0 – 10.0          |

**🔥 Exam tip:** Critical = 9.0–10.0 (fix first). A score like 9.4 is Critical, not just “high.” Remember the ladder: None 0 · Low ≤3.9 · Medium ≤6.9 · High ≤8.9 · Critical ≥9.0.

 **CVE** = "this specific flaw exists and has an ID"

 **CVSS** = "here's the scoring method used to rate severity"

 **NVD** = "here's the database where CVE + CVSS + affected products all live together"

 **ATT&CK** = a totally different axis — not about individual vulnerabilities, but about attacker *behavior and technique*, which may or may not involve a CVE at all (e.g., phishing or credential dumping have no CVE)

4.  **Types of Vulnerabilities (expanded list — confirmed via supplementary source)**

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 51%" />
<col style="width: 24%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><strong>Type</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>Simple Explanation</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>🧠 Easy Way to Remember</strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>Buffer Overflow</p>
</blockquote></td>
<td><blockquote>
<p>Putting more data into memory than it can hold, causing the extra data to overwrite adjacent memory.</p>
</blockquote></td>
<td><blockquote>
<p>🥛 Glass overflows → spills</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Memory Leak</p>
</blockquote></td>
<td><blockquote>
<p>A program failing to release memory it no longer needs, causing memory usage to keep increasing.</p>
</blockquote></td>
<td><blockquote>
<p>🚰 Leaky faucet → memory keeps being used</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>Resource Exhaustion</p>
</blockquote></td>
<td><blockquote>
<p>Using up system resources such as CPU, memory, or connections until the system becomes unavailable.</p>
</blockquote></td>
<td><blockquote>
<p>🔋 Resources run out → system stops</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Integer Overflow</p>
</blockquote></td>
<td><blockquote>
<p>A number becoming too large for its storage space, causing it to wrap around to an unexpected value.</p>
</blockquote></td>
<td><blockquote>
<p>🔢 Number too big → wraps around</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>Null Pointer/Object Dereference</p>
</blockquote></td>
<td><blockquote>
<p>A program trying to access an object or memory location that does not exist, causing an error or crash.</p>
</blockquote></td>
<td><blockquote>
<p>🕳️ Trying to use nothing → crash</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Race Condition</p>
</blockquote></td>
<td><blockquote>
<p>Two processes accessing or changing the same resource at the same time, causing an unexpected result.</p>
</blockquote></td>
<td><blockquote>
<p>🏃 Two processes race → conflict</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>Improper Certificate Validation</p>
</blockquote></td>
<td><blockquote>
<p>A system accepting an invalid, expired, or untrusted certificate as legitimate.</p>
</blockquote></td>
<td><blockquote>
<p>🪪 Fake ID accepted → security problem</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>DLL Injection</p>
</blockquote></td>
<td><blockquote>
<p>Inserting malicious code from a DLL into a running process, allowing an attacker to execute code inside that process.</p>
</blockquote></td>
<td><blockquote>
<p>💉 Inject bad code → control process</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><mark>Use-After-Free</mark></p>
</blockquote></td>
<td><blockquote>
<p>Using a block of memory after it has already been freed/released; a leftover pointer still points to it, causing a crash or attacker code execution.</p>
</blockquote></td>
<td><blockquote>
<p>🗑️ Used the trash after throwing it out</p>
</blockquote></td>
</tr>
</tbody>
</table>

5.  **Vulnerability Assessment Types**

| **Type**                    | **Simple Explanation**                                                                      | **🧠 Easy Way to Remember**           |
|-----------------------------|---------------------------------------------------------------------------------------------|---------------------------------------|
| Active Assessment           | Directly interacting with the target by sending probes or packets to discover weaknesses.   | 🎯 Touch the target → get information |
| Passive Assessment          | Observing network traffic or information without directly interacting with the target.      | 👀 Watch without touching             |
| External Assessment         | Assessing the target from outside the organization, like an external attacker.              | 🌐 Outside → attacker's view          |
| Internal Assessment         | Assessing the target from inside the network, like an insider or compromised user.          | 🏢 Inside → insider's view            |
| Host-based Assessment       | Checking a specific computer or server for configuration, patches, and security weaknesses. | 💻 One host → check security          |
| Application Assessment      | Checking an application for security weaknesses and vulnerabilities.                        | 🌐 Application → find weaknesses      |
| Wireless Network Assessment | Checking a Wi-Fi network for security weaknesses and vulnerabilities.                       | 📶 Wi-Fi → check security             |

**6. Authenticated vs. Unauthenticated Scanning (important real-world nuance)**

| **Type**                 | **Simple Explanation**                                                                                                                     | **🧠 Easy Way to Remember**    |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| **Unauthenticated Scan** | Scanning a system **without credentials**, showing only what an external attacker can discover.                                            | 🕵️ **No login → outside view** |
| **Authenticated Scan**   | Scanning a system **with valid credentials**, allowing the scanner to see internal details such as missing patches and installed software. | 🔑 **Login → inside view**     |

**🔥 Exam tip**

**Unauthenticated = What can an attacker see from outside?**  
**Authenticated = What can the scanner see after logging in?**

👉 **Key difference:**  
**No credentials = limited visibility**  
**Valid credentials = deeper and more accurate visibility**

**7. Key Tools (from your Master Study Guide)**

| **Tool** | **Purpose**                                   | **🧠 Utility / Main Difference**                                                                                                                     |
|----------|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nessus   | Vulnerability scanner by Tenable              | 🔍 General-purpose vulnerability scanning — finds missing patches, misconfigurations, and vulnerabilities across systems, devices, and applications. |
| OpenVAS  | Free/open-source vulnerability scanner        | 🆓 Open-source alternative to Nessus — performs both authenticated and unauthenticated vulnerability scans.                                          |
| Qualys   | Cloud-based vulnerability management platform | ☁️ Cloud/enterprise platform — continuously scans and manages vulnerabilities across large environments, including cloud and on-prem systems.        |
| Nexpose  | Rapid7 vulnerability management tool          | 🎯 Focuses on risk prioritization — helps identify vulnerabilities and prioritize which ones should be fixed first.                                  |
| Nikto    | Web server vulnerability scanner              | 🌐 Web-server focused — checks web servers for known vulnerabilities and security misconfigurations.                                                 |

**🔥 Easy CEH memory**

- **Nessus → 🔍 General vulnerability scanner**

- **OpenVAS → 🆓 Open-source scanner**

- **Qualys → ☁️ Cloud/enterprise vulnerability management**

- **Nexpose → 🎯 Risk prioritization**

- **Nikto → 🌐 Web server scanner**

**Important: Nessus, OpenVAS, Qualys, and Nexpose are mainly broader vulnerability assessment/management tools, while Nikto is specifically focused on web servers.**

**8. V13 AI Addition 🤖**

Your Master Study Guide flags this directly:

**Machine learning for vulnerability assessment** — pattern recognition identifies complex vulnerabilities that conventional scanners miss, improving detection efficiency by up to **40%**.

**Exam angle:** Same "force-multiplier" theme as previous modules — AI here means *faster, smarter pattern recognition* on top of traditional scanning, not a replacement technique. The **40% figure** itself is a plausible direct-recall exam detail.

**9. Vulnerability Management Life Cycle (process, not just a scan)**

| **Step**                      | **Simple Explanation**                                                                                                                                                                               | **🧠 Easy Way to Remember**    |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| **1. Identify Assets**        | Identify all **servers, computers, applications, network devices, and important data** that need protection.                                                                                         | 📋 **What do we have?**        |
| **2. Vulnerability Scan**     | Scan the assets to **discover security weaknesses**, such as missing patches, outdated software, weak configurations, or known vulnerabilities.                                                      | 🔍 **What is vulnerable?**     |
| **3. Risk Assessment**        | Evaluate and **prioritize vulnerabilities** using information such as **CVE** (identifies the vulnerability), **CVSS** (measures its severity), likelihood of exploitation, and **business impact**. | ⚠️ **What is most dangerous?** |
| **4. Remediation**            | Take action to **reduce or eliminate the risk** by installing patches, changing configurations, removing vulnerable software, or accepting the risk.                                                 | 🔧 **How do we fix it?**       |
| **5. Verification**           | **Scan or test again** after remediation to confirm that the vulnerability has actually been fixed.                                                                                                  | ✅ **Did the fix work?**       |
| **6. Monitoring / Reporting** | Continuously **monitor for new vulnerabilities**, track remediation progress, and report the security status.                                                                                        | 📊 **Keep watching**           |

**🎯 WHAT TO MEMORIZE COLD FOR THE EXAM (Module 05)**

1.  **CVE vs. CVSS** — ID/catalog vs. severity score (0-10)

2.  **The 3 official learning objectives**: Summarize concepts, Use tools, Analyze reports

3.  **All vulnerability types**: Buffer Overflow, Memory Leak, Resource Exhaustion, Integer Overflow, Race Condition, Improper Certificate Validation

4.  **Authenticated vs. Unauthenticated scanning** — credentialed scans are more thorough/accurate

5.  **Tool names**: Nessus, OpenVAS, Qualys, Nexpose, Nikto

6.  **V13 AI addition**: ML-based detection, ~40% efficiency improvement

7.  **Vulnerability Management Life Cycle** steps in order

**Exam behavior tip specific to this module:** Expect definition-matching questions for vulnerability types (e.g., a scenario describing memory being used after it was freed → Use-After-Free, which is its own distinct flaw type — not Race Condition or Buffer Overflow) and questions testing whether you understand why authenticated scans are more reliable than unauthenticated ones.

| **Website**                                      | **What is it?**                                   | **Main question it answers**                |
|--------------------------------------------------|---------------------------------------------------|---------------------------------------------|
| [<u>CVE.org</u>](https://www.cve.org/)           | **CVE Program**                                   | **What is the vulnerability ID?**           |
| [<u>NVD</u>](https://nvd.nist.gov/vuln)          | **National Vulnerability Database**               | **How serious is it and what is affected?** |
| [<u>MITRE ATT&CK</u>](https://attack.mitre.org/) | **Adversary tactics & techniques knowledge base** | **How do attackers behave/use techniques?** |

| **CVE** (Common Vulnerabilities and Exposures) |
|------------------------------------------------|
| **CVSS** (Common Vulnerability Scoring System) |
| **NVD** (National Vulnerability Database)      |
