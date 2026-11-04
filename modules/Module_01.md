**Module 01 — Introduction to Ethical Hacking (Easy-to-Understand Summary)**

Think of Module 01 as the "rules of the game" chapter. Before you learn any hacking technique, CEH wants you to understand: what security actually means, who the players are, what order hackers work in, and what laws govern all of this. It's foundational — and it quietly reappears throughout the whole exam because later modules assume you know these basics cold.

**1. What is "Information Security," really?**

Imagine a house. Security means:

- **Confidentiality** — only people with a key can get in (no unauthorized viewing of data)

- **Integrity** — nobody rearranges your furniture without you knowing (data isn't tampered with) Integrity means making sure that data remains accurate, complete, and unchanged unless an authorized person intentionally modifies it.

- **Availability** — the house is accessible when you need it (systems/data are up and usable)

This is the **CIA Triad** — the single most important concept in this module. Everything else builds on it.

Two bonus concepts often layered on:

- **Authenticity** — the person/data is who/what it claims to be

- **Non-repudiation** — you can't later deny you did something (e.g., a signed contract)

**Easy memory trick:** CIA = Confidentiality, Integrity, Availability. If a question describes "data was leaked" → Confidentiality violated. "Data was altered" → Integrity violated. "Server went down" → Availability violated.

**2. The Security vs. Usability Tug-of-War**

The more locks you put on the house, the harder it is for the actual residents to get in and out easily. **Security, Functionality, and Usability form a triangle** — pushing one up pulls another down. Exam may ask you to recognize this trade-off conceptually.

**Easy way to remember**

🔒 **More security** → potentially less convenient  
⚙️ **More functionality** → potentially more attack surface  
😊 **More usability** → potentially weaker security

**3. Who are the "hackers"? (Very testable — memorize this table)**

| **Type**            | **Simple explanation**                                      |
|---------------------|-------------------------------------------------------------|
| **White Hat**       | The "good guy" — hacks with permission to help              |
| **Black Hat**       | The criminal — hacks to harm/profit illegally               |
| **Gray Hat**        | In between — no malicious intent, but no permission either  |
| **Suicide Hacker**  | Doesn't care about getting caught                           |
| **Script Kiddie**   | Uses other people's tools without understanding them        |
| **Hacktivist**      | Hacks for a political/social cause                          |
| **State-Sponsored** | Works for a government                                      |
| **Cyber Terrorist** | Motivated by fear/ideology, targets critical infrastructure |

**⚠️ Don’t confuse the two “phase” lists.** This module teaches the **5 Phases of Hacking** (Recon → Scanning → Gaining Access → Maintaining Access → Clearing Tracks). Later modules follow a different list, the **CEH Hacking Methodology** (Footprinting → Scanning → Enumeration → Vulnerability Analysis → System Hacking). Enumeration and Vulnerability Analysis appear ONLY in the methodology, never in the 5 Phases. If a question says “Phase 4,” check which list it means.

**4. The Five Phases of Hacking (THE most important list in this module — memorize the order)**

Think of it like a burglar planning a robbery:

1.  **Reconnaissance** — scope out the house from outside (gather info)

2.  **Scanning** — check which doors/windows are open (find live systems, ports)

3.  **Gaining Access** — actually break in (exploit a vulnerability)

4.  **Maintaining Access** — leave a spare key hidden (backdoors, rootkits)

5.  **Clearing Tracks** — wipe fingerprints (delete logs, hide evidence)

**Exam trap:** Questions often describe a scenario and ask "which phase is this?" — you must recognize the phase from behavior, not just recite the list.

| **Phase**                 | **Passive / Active?** |
|---------------------------|-----------------------|
| **1. Reconnaissance**     | 🟢 Passive and Active |
| **2. Scanning**           | 🔴 Mostly Active      |
| **3. Gaining Access**     | 🔴 Active             |
| **4. Maintaining Access** | 🔴 Active             |
| **5. Clearing Tracks**    | 🔴 Active             |

**5. Cyber Kill Chain (a more detailed 7-step version, used by defenders)**

**RWDEICA** (memory trick): Recon → Weaponize → Deliver → Exploit → Install → Command & Control (C2) → Actions on Objectives

This is Lockheed Martin's framework — mostly used in defensive/SOC contexts. You just need to recognize the steps and know it maps roughly onto the 5 hacking phases but with more granularity.

Also know (briefly, conceptually):

- **MITRE ATT&CK** — a big reference matrix of real attacker techniques, used for threat intelligence

- **Diamond Model** — Adversary, Capability, Infrastructure, Victim (4 corners of an attack)

**1. The 5 Hacking Phases = Simple attacker journey**

You already have:

1.  **Reconnaissance** → Gather information

2.  **Scanning** → Find weaknesses

3.  **Gaining Access** → Get inside

4.  **Maintaining Access** → Stay inside

5.  **Clearing Tracks** → Hide evidence

Think of it as:

**What does a hacker generally do from beginning to end?**

It's a **simple, high-level model**.

**2. Cyber Kill Chain = More detailed attack lifecycle**

The **Cyber Kill Chain** breaks an attack into **7 stages**:

**Recon → Weaponize → Deliver → Exploit → Install → C2 → Actions**

Let's use the house analogy 🏠.

| **Kill Chain Phase**         | **House Analogy**               | **Meaning**                                      |
|------------------------------|---------------------------------|--------------------------------------------------|
| **1. Reconnaissance**        | Observe the house               | Gather information                               |
| **2. Weaponization**         | Prepare burglary tools          | Create/select the attack capability              |
| **3. Delivery**              | Send the tool to the house      | Get malicious content/access mechanism to target |
| **4. Exploitation**          | Use the weakness                | Trigger or exploit a vulnerability               |
| **5. Installation**          | Get equipment inside            | Establish malicious presence                     |
| **6. Command & Control**     | Communicate with someone inside | Remote control of compromised system             |
| **7. Actions on Objectives** | Steal valuables                 | Achieve the attacker's final goal                |

**Comparison: 5 Phases vs Cyber Kill Chain**

Here is the easiest mapping:

| **5 Hacking Phases**   | **Cyber Kill Chain**                                     |
|------------------------|----------------------------------------------------------|
| **Reconnaissance**     | Reconnaissance                                           |
| **Scanning**           | Mostly part of Recon / preparation                       |
| **Gaining Access**     | Weaponization → Delivery → Exploitation                  |
| **Maintaining Access** | Installation → Command & Control                         |
| **Clearing Tracks**    | Can happen throughout; not explicitly a Kill Chain phase |
| **—**                  | Actions on Objectives                                    |

**Where does MITRE ATT&CK fit?**

The Kill Chain tells you: **WHEN in the attack lifecycle something happens.**

MITRE ATT&CK tells you: **HOW attackers perform actions.**

Think of it as a huge knowledge base of attacker behaviours and techniques.

For example:

CYBER KILL CHAIN

↓

Exploitation

↓

MITRE ATT&CK

↓

Which techniques could attackers use here?

MITRE ATT&CK contains many tactics and techniques such as:

- Phishing

- Credential access

- Command execution

- Persistence

- Lateral movement

- Data collection

- Exfiltration

So:

**Kill Chain = attack timeline**

**MITRE ATT&CK = attacker technique encyclopedia**

**Where does the Diamond Model fit? 💎**

The Diamond Model looks at an attack as **four connected elements**:

<img src="media/image1.png" style="width:2.11122in;height:1.58341in" />

**The four questions:**

| **Element**        | **Question**                           |
|--------------------|----------------------------------------|
| **Adversary**      | Who is attacking?                      |
| **Capability**     | What tools/malware/skills do they use? |
| **Infrastructure** | What systems/domains/IPs do they use?  |
| **Victim**         | Who or what is being attacked?         |

Example:

A threat group (**Adversary**) uses ransomware (**Capability**) hosted or controlled through servers/domains (**Infrastructure**) against a company (**Victim**).

So the Diamond Model is useful for **analyzing and connecting information about an intrusion**.

**6. Testing Types — Black/White/Gray Box (very commonly tested)**

Imagine hiring someone to test if your house is secure:

- **Black Box** — they know nothing, just like a real burglar (no prior info)

- **White Box** — they have blueprints and keys (full internal knowledge)

- **Gray Box** — they know a little (partial knowledge, like a former tenant)

**7. Security Controls & Risk Management**

- **Defense in Depth** — multiple layers of security (like a house with a fence, alarm, AND locked doors — not just one)

- **Risk Management steps**: Identify → Assess → Treat → Track → Review

- **Control types**: Physical (locks, guards), Technical (firewalls, encryption), Administrative (policies, training)

- **SOC basics**: Tier 1 (monitors alerts) → Tier 2 (investigates) → Tier 3 (deep expert response)

**8. Laws & Standards (match name → purpose, not deep legal detail)**

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Law/Standard</strong></th>
<th><strong>What it protects</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>HIPAA</strong></td>
<td>Healthcare data (US)</td>
</tr>
<tr class="even">
<td><strong>SOX</strong></td>
<td>Financial reporting integrity (US)</td>
</tr>
<tr class="odd">
<td><strong>GDPR</strong></td>
<td>Personal data privacy (EU)</td>
</tr>
<tr class="even">
<td><strong>PCI-DSS</strong></td>
<td>Payment card data (global)</td>
</tr>
<tr class="odd">
<td><strong>ISO/IEC 27001</strong></td>
<td>Info security management systems</td>
</tr>
<tr class="even">
<td><strong>DMCA</strong></td>
<td>Copyright (US)</td>
</tr>
<tr class="odd">
<td><strong>CFAA</strong></td>
<td><p><strong>Computer Fraud and Abuse Act</strong> — the main US law that criminalizes unauthorized computer access; this is the one CEH references most for the <em>legal boundary of ethical hacking</em></p>
<p><strong>CFAA</strong> — the anchor law tying "ethical" hacking to legal boundaries; unauthorized access = illegal even if well-intentioned</p></td>
</tr>
</tbody>
</table>

**🎯 WHAT TO MEMORIZE COLD FOR THE EXAM (Module 01)**

If you remember nothing else, know these by heart:

1.  **CIA Triad** — definitions + ability to spot which one is violated in a scenario

2.  **The 5 Phases of Hacking, in exact order**: Recon → Scan → Gain Access → Maintain Access → Clear Tracks

3.  **Hacker types** — especially White/Black/Gray Hat and Hacktivist vs. Suicide Hacker vs. Script Kiddie

4.  **Black/White/Gray Box testing** — which one = "no knowledge," which = "full knowledge"

5.  **Cyber Kill Chain 7 steps** (recognize the order, even if you don't need every detail)

6.  **CFAA** — the anchor law tying "ethical" hacking to legal boundaries; unauthorized access = illegal even if well-intentioned

7.  Match each **law/standard to its domain** (HIPAA=health, PCI-DSS=payment cards, GDPR=EU privacy, SOX=finance)

**Exam behavior tip specific to this module:** EC-Council loves writing a short scenario ("An employee tests a system without written authorization...") and asking you to identify the violation, hacker type, or phase. Practice recognizing concepts *in context*, not just definitions in isolation.

**— ADDENDUM: Missing Sections Added (to fully match official CEH v13 Module 01 blueprint) —**

**9. Threat vs. Vulnerability vs. Risk vs. Attack (classic exam distinction)**

Back to the house:

- **Vulnerability —** a **weakness** in the house (a window without a lock)

- **Threat —** a **potential danger that** could exploit that weakness (a burglar in the neighborhood)

- **Risk — the likelihood x impact** if the threat exploits the vulnerability (how likely is it this burglar breaks in through that window, and how bad would it be?)

- **Attack —** the **actual** act of exploitation (the burglar climbs through the window)

Exam trap: questions describe a scenario and ask you to label which of these four terms applies -- read carefully, they are often used interchangeably in casual English but mean very different things on the exam.

**10. Types of Attacks (classification)**

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 41%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Attack Type</strong></th>
<th><strong>House Analogy</strong></th>
<th><strong>Meaning</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Passive Attack</strong></td>
<td>Watching the house from across the street</td>
<td>Attacker observes/intercepts data without altering it (e.g., sniffing, eavesdropping)</td>
</tr>
<tr class="even">
<td><strong>Active Attack</strong></td>
<td>Breaking a window to get in</td>
<td>Attacker directly modifies/disrupts data or systems (e.g., DoS, session hijacking)</td>
</tr>
<tr class="odd">
<td><strong>Insider Attack</strong></td>
<td>A family member misuses their own house key</td>
<td>Launched by someone with authorized/internal access (employee, contractor)</td>
</tr>
<tr class="even">
<td><strong>Outsider Attack</strong></td>
<td>A stranger breaks in from outside</td>
<td>Launched by someone with no authorized access to the network</td>
</tr>
<tr class="odd">
<td><strong>Close-in Attack</strong></td>
<td>Someone standing close enough to peek at your code entry</td>
<td>Attacker is in physical proximity to the target (shoulder surfing, dumpster diving)</td>
</tr>
<tr class="even">
<td><strong>Distribution Attack</strong></td>
<td><p>A rigged lock installed at the factory before you even bought the house</p>
<p>Distribution Attack = Attack during the supply/distribution chain, before the product reaches the target.</p></td>
<td>Attacker tampers with hardware/software during manufacturing or distribution, before it reaches the target</td>
</tr>
</tbody>
</table>

**11. Attack Vectors (high-level overview)**

An attack vector is simply the path or means an attacker uses to break into the house. CEH v13 wants you to recognize these categories at a high level (deep dives happen in later modules):

- **Cloud computing threats —** misconfigured storage, weak IAM

- **APTs (Advanced Persistent Threats) —** long-term, stealthy, targeted intrusions

- **Viruses, worms, ransomware —** malware-based attack vectors

- **Mobile threats —** malicious apps, insecure app stores

- **IoT threats —** weakly secured smart devices as entry points

- **Insider threats —** malicious or negligent employees

- **Supply chain attacks —** compromising a trusted third-party vendor to reach the real target

- **AI-based threats (V13 addition) —** prompt injection, AI-generated malware, deepfake-driven social engineering

**12. Information Warfare**

A named concept in the official curriculum, usually tested at a conceptual level -- think of it as "war fought with information instead of weapons," split into two sides:

- **Defensive Information Warfare —** protecting your own information systems (e.g., a nation defending its infrastructure)

- **Offensive Information Warfare —** attacking an adversary's information systems (e.g., propaganda, espionage, sabotage of enemy networks)

**13. V13-Specific AI Addition: LLM/ChatGPT in Hacking (Important -- flagged in your own Master Study Guide under Module 1!)**

CEH v13 is marketed as the world's first **AI-powered ethical hacking certification**, and this topic is explicitly mapped to Module 1 in the official blueprint. Know these terms conceptually:

- **Prompt Injection —** manipulating an AI/LLM's input to make it bypass its own safety instructions or reveal restricted behavior

- **AI-Assisted Exploit Writing —** attackers using LLMs to help generate or refine malicious code/scripts faster

- **Jailbreaking (of an AI model) —** techniques used to trick an AI system into ignoring its built-in restrictions

Easy memory trick: think of an LLM like a new employee with a strict rulebook (its safety training). Prompt injection = tricking the employee with clever wording. Jailbreaking = trying multiple tricks until the employee breaks the rulebook entirely.

**14. Skills & Scope/Limitations of an Ethical Hacker**

What makes someone qualified, and what boundaries they must respect:

- **Technical skills —** networking, OS internals, programming/scripting, knowledge of security tools

- **Non-technical skills —** strong ethics, ability to follow the law, good communication/reporting skills, ability to keep findings confidential

- **Scope of engagement —** exactly what systems/networks the ethical hacker is authorized to test, defined in writing before any testing begins

- **Limitations —** an ethical hacker may NOT go beyond the agreed scope, may NOT use the access for personal gain, and remains bound by the same laws (e.g., CFAA) as anyone else if they step outside the authorized boundary

**Addendum to "What to Memorize Cold" -- additional items from this update**

8.  **Threat vs Vulnerability vs Risk vs Attack —** know all four definitions and be able to label a scenario correctly

9.  **Types of Attacks —** Passive vs Active, Insider vs Outsider, Close-in, Distribution

10. **V13 AI Addition —** Prompt Injection, AI-assisted exploit writing, Jailbreaking (mapped directly to Module 1 in the official blueprint)

11. **Information Warfare —** Defensive vs Offensive
