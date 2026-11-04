**Module 02 — Footprinting & Reconnaissance**

Continuing the house-burglary analogy: if Module 01 taught you the rules of the game, Module 02 is where the burglar actually starts casing the house — walking past it, checking the mailbox, asking neighbors questions — all without ever touching the property. This is the first of the 5 Hacking Phases, and it's one of the highest-weighted domains on the real exam (17%, tied with Modules 3 & 4 under "Reconnaissance Techniques").

# **1. What is Footprinting? 🏠**

Footprinting = gathering as much information as possible about a target before making any contact that could be detected. Think of it as reading everything publicly available about the house — property records, social media of the residents, delivery schedules — without ever knocking on the door.

**Goal:** Build a complete profile of the target: network ranges, domain names, employee names, technologies used, physical locations, and more.

# **1a. Reconnaissance vs. Footprinting — Are They the Same Thing? (Important clarification)**

This is one of the most confused points for CEH students, so let's settle it with a direct quote from your own Matt Walker CEH Study Guide:

*"In earlier CEH versions, the first phase was listed as reconnaissance, but in the current version, the first step... is footprinting."*

*"Footprinting is nothing more than the steps taken to gather evidence and information on the targets you want to attack. It can be passive or active in nature: passive footprinting gathers information without any direct interaction on the part of the attacker, whereas active footprinting does require direct interaction."*

**— Matt Walker, CEH Study Guide**

**What this means in plain terms:** "Reconnaissance" and "Footprinting" are NOT two different concepts layered on top of each other. They are the SAME Phase 1 of the 5 Hacking Phases — just renamed across CEH exam versions.

| **Term**           | **When Used**                                                        | **What It Refers To**                                             |
|--------------------|----------------------------------------------------------------------|-------------------------------------------------------------------|
| **Reconnaissance** | Older CEH exam versions' official name for Phase 1                   | Phase 1 of the 5 Hacking Phases — gathering info about the target |
| **Footprinting**   | Current CEH exam versions' official name for Phase 1 (including v13) | Same Phase 1 — identical concept, updated terminology             |

**The one real subdivision that DOES exist** is within footprinting/reconnaissance itself — the Passive vs. Active split covered in the next section. There is no separate "official active version" of reconnaissance distinct from footprinting — **Active Footprinting** IS the correct term for the active side, confirmed directly by your study guide.

**Official EC-Council courseware adds a useful nuance** (from official CEH courseware material):

*"Footprinting refers to the process of collecting information about a target network and its environment, which helps in evaluating the security posture of the target organization's IT infrastructure."*

*"Reconnaissance refers to collecting information about a target, which is the first step in any attack on a system."*

**— EC-Council official courseware**

**How to reconcile both sources:** Walker's point (they're the same Phase 1, renamed across versions) and EC-Council's phrasing (subtly different emphasis) are not in conflict — they're just answering slightly different questions:

| **Term**           | **Official Emphasis**                                                                            | **Scope**                                                           |
|--------------------|--------------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| **Reconnaissance** | The general, universal FIRST STEP of any attack, on any type of target                           | Broadest scope — applies to any attack methodology, not IT-specific |
| **Footprinting**   | Specifically gathering info about a target NETWORK/IT ENVIRONMENT to assess its security posture | Narrower, IT/network-focused application of reconnaissance          |

***Bottom line for the exam:** Think of Reconnaissance as the general concept/first phase of ANY attack, and Footprinting as that same concept specifically applied to gathering network/IT infrastructure information. In CEH's context (which is entirely about IT systems), the two are used interchangeably 95% of the time — but if a question is testing precise definitions, Reconnaissance = the broader/first-step term, Footprinting = the IT-environment-specific application of it.*

***Exam tip:** If a question asks you to name "Phase 1" of the hacking phases, either "Reconnaissance" or "Footprinting" should be treated as acceptable/equivalent — don't overthink it as two separate phases.*

# **2. Passive vs. Active Reconnaissance (the core split of this module)**

| **Type**          | **House Analogy**                                                                         | **Meaning**                                                                                   | **Detection Risk**                    |
|-------------------|-------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|---------------------------------------|
| **Passive Recon** | 🟢 Observing the house from across the street — public records, mail, neighbors gossiping | Gathering info from public sources without directly interacting with target systems           | Very low — target usually never knows |
| **Active Recon**  | 🔴 Walking up and testing the doorknob                                                    | Directly interacting with target systems (e.g., pinging, visiting their website while logged) | Higher — can trigger logs/alerts      |

**Exam trap:** A scenario might describe "checking a company's public job postings for tech stack clues" (passive) vs. "sending a ping to their server" (active) — you need to correctly classify the technique, not just know the definition.

Active footprinting interacts with the target to gather info; scanning probes it to find what's open and running.

# **3. Types of Footprinting (the categories EC-Council tests)**

| **Category**                        | **House Analogy**                                       | **What You're Gathering**                                             | **Most Important Tools**                                    |
|-------------------------------------|---------------------------------------------------------|-----------------------------------------------------------------------|-------------------------------------------------------------|
| **DNS Footprinting**                | 🏠 Checking the house's **address records**             | Domain records, subdomains, mail servers                              | **nslookup, dig, DNSDumpster, WHOIS**                       |
| **Network Footprinting**            | 🗺️ Mapping the **street and neighborhood**              | IP ranges, network structure/topology                                 | **Traceroute/Tracert, Nmap, Netcraft**                      |
| **Website Footprinting**            | 👀 Looking at the house's **signs and visible details** | Technologies, source-code comments, directories                       | **WhatWeb, Wappalyzer, Netcraft, Google Dorks**             |
| **Email Footprinting**              | ✉️ Looking at the **information on the mail**           | Email headers, sender IPs, mail-server information                    | **Email Header Analyzer, MXToolbox, WHOIS**                 |
| **Social Engineering Footprinting** | 🗣️ **Talking to the neighbors**                         | Information obtained from people                                      | **Social media, LinkedIn, Maltego, SET**                    |
| **Competitive Intelligence**        | 🔭 **Watching a rival house**                           | Competitor business information, plans, market position               | **Google, LinkedIn, company websites, Crunchbase**          |
| **OSINT**                           | 📚 Using **everything publicly available**              | Information from websites, social media, forums, public records, etc. | **Maltego, theHarvester, SpiderFoot, Google Dorks, Shodan** |

# **4. Key Tools (from your Master Study Guide + Nmap Cookbook)**

| **Tool**           | **Purpose**                                                                                                                          | **Passive/Active** |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **WHOIS**          | Domain registration info (owner, registrar, dates)                                                                                   | 🟢 Passive         |
| **Nslookup / Dig** | Query DNS records (A, MX, NS). Passive against public resolvers; ACTIVE if aimed at the target's own DNS or used for a zone transfer | 🟢 Passive         |
| **theHarvester**   | Gathers emails, subdomains, names from public sources                                                                                | 🟢 Passive         |
| **Maltego**        | Visualizes OSINT relationships (people, domains, infrastructure)                                                                     | 🟢 Passive         |
| **Recon-ng**       | Web reconnaissance framework, modular OSINT                                                                                          | 🟢 Passive         |
| **Shodan**         | Search engine for internet-connected devices (finds exposed servers/IoT)                                                             | 🟢 Passive         |
| **Google Dorks**   | Advanced Google search operators to find exposed files/info                                                                          | 🟢 Passive         |

**Easy memory trick:** Most OSINT-style footprinting is passive — that's the core idea. But watch out: some footprinting is ACTIVE because it touches the target's own systems — DNS zone transfer, querying the target's own DNS servers, traceroute, email tracking, and social engineering all count as active footprinting. So don't answer “footprinting = always passive.” Full port scanning and banner grabbing start in Module 03.

# **5. Google Dorking — a favorite exam topic**

Google Dorks use special search operators to surface information not meant to be easily found:

| **Operator**  | **Purpose**                    | **Example**               |
|---------------|--------------------------------|---------------------------|
| **site:**     | Restrict search to a domain    | site:example.com          |
| **filetype:** | Search for specific file types | filetype:pdf confidential |
| **intitle:**  | Search page titles             | intitle:"index of"        |
| **inurl:**    | Search within URLs             | inurl:admin               |

# **6. DNS Record Types Worth Knowing**

For example, imagine the domain **example.com**.

| **Record** | **Very simple meaning**                                   | **Example**                             |
|------------|-----------------------------------------------------------|-----------------------------------------|
| **A**      | 🏠 Website name → **IPv4 address**                        | example.com → 192.168.1.10              |
| **AAAA**   | 🏠 Website name → **IPv6 address**                        | example.com → IPv6 address              |
| **MX**     | 📧 **Where should email go?**                             | example.com → mail.example.com          |
| **NS**     | 📖 **Who manages DNS for this domain?**                   | ns1.example.com                         |
| **CNAME**  | 🔗 **Another name for the same thing**                    | www.example.com → example.com           |
| **TXT**    | 📝 **Extra text/information**                             | SPF, domain verification, etc.          |
| **SOA**    | 👑 **Main administrative information about the DNS zone** | Primary DNS server + admin/zone details |

# **7. V13 AI Addition 🤖**

Your Master Study Guide flags this directly under Module 2:

**AI-powered Reconnaissance** — AI/ML tools automate OSINT and target profiling, correlating data from multiple sources simultaneously (faster than a human doing it manually).

***Exam angle:** expect a question testing whether you understand that AI is now used to automate and speed up traditional OSINT — not a fundamentally new technique, but a force-multiplier on old ones.*

# **8. Footprinting Countermeasures (defender's side — often tested too)**

- Limit publicly available employee/organizational info (LinkedIn oversharing, job posts revealing tech stack)

- Configure DNS servers to prevent zone transfers

- Use privacy protection on WHOIS registration

- Educate employees on social engineering/OSINT risks

- Monitor for company data appearing in breach dumps or paste sites

# **🎯 WHAT TO MEMORIZE COLD FOR THE EXAM (Module 02)**

1.  **Passive vs. Active Recon** — be able to classify a described technique correctly

2.  **All 7 footprinting categories** (DNS, Network, Website, Email, Social Engineering, Competitive Intelligence, OSINT) and what each gathers

3.  **Tool-to-purpose mapping**: theHarvester (emails), Maltego (visualization), Shodan (exposed devices), WHOIS (domain registration), Recon-ng (framework)

4.  **Google Dork operators**: site:, filetype:, intitle:, inurl:

5.  **DNS record types**: A, AAAA, MX, NS, CNAME, TXT, SOA — know what each one returns

6.  **V13 AI Addition**: AI-powered reconnaissance = automated OSINT correlation

7.  **Recognize that footprinting is almost entirely passive** — this is the exam's core distinction from Module 03 (Scanning), which is active

**Exam behavior tip specific to this module:** Expect "which tool would you use to find X" questions (e.g., "find email addresses associated with a domain" → theHarvester) and scenario questions asking you to identify whether a described action counts as passive or active reconnaissance.
