**CEH v13 — Exam-Focused Summary (v3 style)**

# Module 07 — Malware Threats

If System Hacking was the burglar breaking in by hand, **malware is the burglar’s automated helper** — a piece of software that sneaks in, does the dirty work, and often keeps working long after the attacker has gone. Per your Master Study Guide this is part of the **System Hacking Phases & Attack Techniques** domain (~15% of the exam), and “malware types” is flagged as a HIGH-density topic — expect several questions.

*Source: CEH v13 Master Study Guide (Drive) + Mr-Infect 07 - Malware threats notes*

## 1. What is Malware? 🦠

**Malware** = “malicious software.” Any program written to damage, steal, spy on, or take control of a system without the owner’s consent. Viruses, worms, Trojans, ransomware, spyware, rootkits — they are all just different **types** of malware.

**House analogy:** Malware is like a robotic burglar you mail to a house. Some kinds need the owner to open the box (virus/Trojan); some crawl in by themselves (worm); some lock every room and demand cash (ransomware); some hide in the walls forever (rootkit).

## 2. Malware Types (memorize this table — highest yield) 🎯

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 47%" />
<col style="width: 28%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Type</strong></th>
<th><strong>Key characteristic</strong></th>
<th><strong>🧠 Remember</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Virus</strong></td>
<td>Attaches to a file; needs USER ACTION to spread</td>
<td>🤝 Needs a human to click</td>
</tr>
<tr class="even">
<td><strong>Worm</strong></td>
<td>Self-replicates across networks with NO user action</td>
<td>🐛 Spreads on its own</td>
</tr>
<tr class="odd">
<td><strong>Trojan</strong></td>
<td>Disguised as legitimate software</td>
<td>🐴 Looks safe, hides evil</td>
</tr>
<tr class="even">
<td><strong>Ransomware</strong></td>
<td>Encrypts files, demands payment</td>
<td>🔒 Pay to unlock</td>
</tr>
<tr class="odd">
<td><strong>Rootkit</strong></td>
<td>Hides its presence deep in the OS</td>
<td>👻 Invisible intruder</td>
</tr>
<tr class="even">
<td><strong>Spyware</strong></td>
<td>Secretly monitors the user</td>
<td>🕵️ Watches quietly</td>
</tr>
<tr class="odd">
<td><strong>Keylogger</strong></td>
<td>Records keystrokes</td>
<td>⌨️ Logs what you type</td>
</tr>
<tr class="even">
<td><strong>Adware</strong></td>
<td>Forces unwanted ads</td>
<td>📢 Ad spam</td>
</tr>
<tr class="odd">
<td><strong>Botnet</strong></td>
<td><p>Network of infected “bot” machines, C2 via IRC/HTTP</p>
<p><strong>Botnet = many hacked devices working together.</strong></p></td>
<td>🤖 Zombie army</td>
</tr>
<tr class="even">
<td><strong>Fileless malware</strong></td>
<td>Runs in memory only — no file on disk</td>
<td>💨 Ghost in the RAM</td>
</tr>
<tr class="odd">
<td><strong>APT</strong></td>
<td>Advanced Persistent Threat — long-term, stealthy, targeted</td>
<td>🎯 Slow, patient, hidden</td>
</tr>
</tbody>
</table>

**🖱️ Human action:  
Virus + Trojan → think "user must do something"**

**🤖 Works alone:  
Worm → think "self-replicating / no user action"**

**🔥 The \#1 exam trap — Virus vs Worm:** A **virus needs the user to do something** (open a file, run a program). A **worm spreads by itself** across the network with no help. If the question says “spread automatically / no user interaction” → WORM.

*Source: CEH v13 Master Study Guide (Drive) + Mr-Infect 07 notes*

## 3. Components of Malware (what’s inside the box)

CEH likes to test the “parts” of a malware package. Know what each piece does:

| **Component**  | **Job**                                             |
|----------------|-----------------------------------------------------|
| **Payload**    | The part that does the actual damage when triggered |
| **Dropper**    | Covertly carries and installs the malware           |
| **Downloader** | Fetches additional malware from the internet        |
| **Injector**   | Injects the malicious code into a running process   |
| **Exploit**    | The part that abuses a vulnerability to get in      |
| **Obfuscator** | Hides/scrambles the code from analysts              |
| **Packer**     | Compresses/encrypts the code so it’s unreadable     |
| **Crypter**    | Encrypts the malware to evade antivirus             |

**📌 Dropper vs Downloader:** a **dropper** already CONTAINS the malware and drops it. A **downloader** carries nothing itself — it DOWNLOADS the malware from the internet.

**Metasploit** is a security-testing framework that can exploit vulnerabilities and deliver payloads, while **malware** is malicious software designed to harm or compromise systems.

Think of malware as a **package trying to enter and operate on a computer**:

1.  **Exploit** → finds a weakness and uses it to get in.

2.  **Dropper** → if it already carries the malware, it releases/install it.

3.  **Downloader** → if it doesn't carry the malware, it connects to the internet and downloads it.

4.  **Injector** → puts malicious code into a running process so it can operate there.

5.  **Payload** → performs the actual malicious action.

6.  **Obfuscator / Packer / Crypter** → hide or disguise the malware so security tools and analysts have a harder time recognizing it.

**🧠 Easy story**

**Exploit opens the door → Dropper/Downloader brings the malware → Injector puts it into a process → Payload does the job → Obfuscator/Packer/Crypter hide it.**

## 4. Viruses — Types You Must Recognize

A **virus** is self-replicating code that attaches itself to files (.exe, .com, .bat, .ovl) and needs a user to run the host to spread. CEH tests specific virus types by their behavior:

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Virus type</strong></th>
<th><strong>What makes it special</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Boot sector</strong></td>
<td>Infects the master boot record; <strong>runs before the OS loads</strong></td>
</tr>
<tr class="even">
<td><strong>File virus</strong></td>
<td>Infects executable files / Attaches to <strong>executable files</strong> like .exe</td>
</tr>
<tr class="odd">
<td><strong>Macro virus</strong></td>
<td>Written in VBA; <strong>hides</strong> in <strong>Word/Excel docs</strong>; spreads via email</td>
</tr>
<tr class="even">
<td><strong>Multipartite</strong></td>
<td>Infects BOTH boot sector and files</td>
</tr>
<tr class="odd">
<td><strong>Polymorphic</strong></td>
<td>Changes its code each time but keeps the same function (evades signatures) <strong>Changes its appearance/code</strong> each time to avoid detection</td>
</tr>
<tr class="even">
<td><strong>Metamorphic</strong></td>
<td>Completely rewrites itself each infection (even harder to detect). <strong>Rewrites its code</strong> each time it infects a system</td>
</tr>
<tr class="odd">
<td><strong>Stealth / Tunneling</strong></td>
<td><p>Intercepts AV requests to hide the changes it made</p>
<p><strong>Hides its presence/changes</strong> from antivirus</p></td>
</tr>
<tr class="even">
<td><strong>Encryption virus</strong></td>
<td><p>Encrypts its own body with a changing key to hide</p>
<p><strong>Encrypts its own code</strong> to make analysis/detection harder</p></td>
</tr>
<tr class="odd">
<td><strong>Sparse infector</strong></td>
<td><p>Infects only occasionally (e.g. every 10th run) to stay quiet</p>
<p>Infects files <strong>only sometimes</strong>, not every time</p></td>
</tr>
<tr class="even">
<td><strong>Cavity virus</strong></td>
<td><p>Fills empty space in a file so file size doesn’t change</p>
<p>Hides inside <strong>unused space</strong> in a file without increasing its size</p></td>
</tr>
<tr class="odd">
<td><strong>Companion virus</strong></td>
<td><p>Creates a same-named file that runs instead of the real one</p>
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
</tbody>
</table>
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Creates a <strong>different file with the same name</strong> that gets executed instead</th>
</tr>
</thead>
<tbody>
</tbody>
</table></td>
</tr>
<tr class="even">
<td><strong>Logic bomb</strong></td>
<td><p>Triggers on a specific date/event (a “time bomb”)</p>
<p>Activates when a <strong>specific condition/event</strong> happens</p></td>
</tr>
</tbody>
</table>

**🎯 Polymorphic vs Metamorphic (classic trap):** Polymorphic = **encrypts/changes appearance** but the underlying code is the same. Metamorphic = **rewrites the whole code** each time. Metamorphic is the more advanced/harder to catch.

A virus can **make copies of itself** but normally needs some **human action** (such as running/sharing an infected file) to help those copies spread.

A worm can **make copies AND spread them automatically** across systems/networks

*Source: Mr-Infect 07 notes (virus types)*

## 5. Virus Lifecycle (the 4 stages)

**Design → Replication → Launch → Detection/Execution**. The virus is created, copies itself, activates when the user triggers it, and eventually its damage (or an AV signature) reveals it. Two phases matter most: the **Infection phase** (spreads) and the **Attack phase** (delivers the payload).

| **Malware package / component** | **Virus lifecycle** | **What happens in the virus case** |
|-----------------------------------------------------------|-----------------------------------------------|--------------------------------------------------------------|
| **Exploit**                                               | **Before / leading into Launch**              | Exploit may be used to get the virus into the target         |
| **Dropper**                                               | **Launch**                                    | May install/drop the virus or another component              |
| **Downloader**                                            | **Launch**                                    | May download the virus/additional malicious code             |
| **Injector**                                              | **Launch → Execution**                        | May place malicious code into a running process              |
| **Replication**                                           | **Replication**                               | 🦠 Virus copies itself → **Infection phase**                 |
| **Payload**                                               | **Detection / Execution**                     | 💥 Payload executes → **Attack phase**                       |
| **Obfuscator**                                            | **Design**                                    | Hides/scrambles the virus code                               |
| **Packer**                                                | **Design**                                    | Packs the virus code                                         |
| **Crypter**                                               | **Design**                                    | Encrypts the virus code                                      |

## 6. Trojans — Types by Purpose

A **Trojan** pretends to be something useful but hides malicious code. It runs at the **same privilege level as the user** who ran it. CEH tests Trojan types by what they DO:

 🦠 **Virus** → when they want to **spread automatically** from file to file/system to system.

 🐴 **Trojan** → when they want to **trick the victim into running/installing** the malware.

| **Trojan Type** | **Purpose**             | **Simple Purpose** |
|-------------------------------------------|---------------------------------------------------|----------------------------------------------|
| **RAT (Remote Access Trojan)**            | Full remote control of the victim (e.g. njRAT)    | **Control victim**                           |
| **Backdoor**                              | Secret re-entry point, bypasses IDS/firewall      | **Come back secretly**                       |
| **Botnet Trojan**                         | Enlists the machine into a bot army (IRC/HTTP C2) | **Make victim a bot**                        |
| **Rootkit Trojan**                        | Hides deep, part of blended attacks               | **Hide malware**                             |
| **Banking / e-banking**                   | Steals bank credentials before encryption         | **Steal bank data**                          |
| **POS Trojan**                            | Targets point-of-sale card readers                | **Steal card data**                          |
| **Defacement Trojan**                     | Changes website content/appearance                | **Change website**                           |
| **Proxy Server Trojan**                   | Turns the victim into a proxy for the attacker    | **Use victim as proxy**                      |
| **Command Shell Trojan**                  | Gives a remote command shell (e.g. Netcat)        | **Run commands remotely**                    |
| **HTTP/HTTPS Trojan**                     | Tunnels out over 80/443 to bypass firewalls       | **Communicate through web**                  |

**📌 Exam cue:** “remote control of a victim machine” → RAT. “secret way back in” → backdoor. “Netcat giving a shell” → command shell Trojan.

## 7. How Attackers Deploy Malware (the toolkit words)

| **Term**             | **Meaning**                                                                   |
|----------------------|-------------------------------------------------------------------------------|
| **Wrapper / Binder** | Binds the Trojan to a legit .exe so it looks genuine                          |
| **Crypter**          | Encrypts the malware to evade antivirus                                       |
| **Dropper**          | Installs the malware silently in the background                               |
| **Exploit kit**      | Automated tool that abuses software vulnerabilities (e.g. Adobe Flash/Reader) |

| **Stage** | **Term(s)** | **What happens**          |
|-------------------------------------|---------------------------------------|-----------------------------------------------------|
| 🟢 **1. GET IN**                    | **Exploit / Exploit Kit**             | Uses a vulnerability to gain entry                  |
| 🟡 **2. DELIVER**                   | **Dropper / Downloader**              | Drops/installs the malware or downloads it          |
| 🔵 **3. RUN**                       | **Injector**                          | Injects malicious code into a running process       |
| 🔴 **4. ACT**                       | **Payload**                           | Performs the actual malicious action                |
| ⚪ **Supporting / Any stage**       | **Obfuscator / Packer / Crypter**     | Hides or disguises the malware                      |
| ⚪ **Supporting / Delivery**        | **Wrapper / Binder**                  | Combines malware with another legitimate executable |

## 

## 8. Ransomware

**Ransomware** encrypts the victim’s files and demands payment (usually cryptocurrency) for the decryption key. Known families to recognize: **WannaCry, LockBit, Conti, Cerber, Ryuk, Maze, Petya/NotPetya**.

**📌 Exam point:** WannaCry spread using the **EternalBlue** exploit (SMBv1, port 445) — that’s why it acted like a worm. Best defense against ransomware = **offline backups**.

## 9. Modern Threats: Fileless, APT & AI Malware

| **Threat**           | **What to know**                                                                                                                                      |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Fileless malware** | Runs in RAM/memory only, no file on disk. Abuses trusted tools (PowerShell, WMI, macros) — called “Living off the Land” (LOL). Very hard to detect.   |
| **APT**              | Advanced Persistent Threat — a long-term, well-funded, stealthy, TARGETED intrusion (often nation-state). Goal: stay hidden and steal data over time. |
| **AI-based malware** | V13 addition — uses ML to self-mutate and evade signature detection (e.g. BlackMamba). Also WormGPT/FraudGPT for AI-written phishing.                 |

**🎯 Fileless cue:** if a question mentions **PowerShell, WMI, “no file on disk,” or “living off the land”** → fileless malware. If it mentions **long-term, stealthy, targeted, nation-state** → APT.

*Source: CEH v13 Master Study Guide (Drive, V13 AI addition) + Mr-Infect 07 notes*

## 10. Malware Analysis — Static vs Dynamic (very testable)

|                  | **Static Analysis**                                    | **Dynamic Analysis**                                         |
|------------------|--------------------------------------------------------|--------------------------------------------------------------|
| Run the malware? | NO — examine without running                           | YES — run it and watch behavior                              |
| Also called      | Code analysis                                          | Behavioral analysis                                          |
| What you do      | Read code, strings, hashes, imports                    | Run in a sandbox, watch files/registry/network               |
| Example tools    | Strings, PEiD, Detect It Easy, IDA, Ghidra, VirusTotal | Cuckoo Sandbox, Process Monitor, Wireshark, Regshot, TCPView |

**📌 Key terms: Sheep Dip** = an isolated, dedicated computer used to test suspicious files before they touch the real network. **Sandbox** = a safe virtual environment to run malware (Cuckoo). **VirusTotal** = online multi-engine scanner.

**Static keywords:** “without executing,” strings, hashing (fingerprinting), disassembly (IDA/Ghidra), packing detection (PEiD/DIE).

**Dynamic keywords:** “execute and observe,” sandbox, Process Monitor, netstat/TCPView, registry monitoring, Wireshark.

## 11. Virus Detection Methods

Malware detection techniques used by antivirus/anti-malware software.

| **Method**             | **How it detects**                                                   | **How it works**                                                      |
|------------------------|----------------------------------------------------------------------|-----------------------------------------------------------------------|
| **Signature scanning** | Matches against a database of known malware patterns                 | Looks for a **known malware pattern**                                 |
| **Integrity checking** | Compares files against a known-good baseline (checksums)             | Checks whether a **file has been changed**                            |
| **Heuristic analysis** | Looks for suspicious behavior/structure — can catch unknown/zero-day | Looks for **suspicious code or behavior**, even if malware is unknown |
| **Interception**       | Watches for suspicious system requests (e.g. logic bombs)            | **Watches actions** made by programs and can stop suspicious ones     |
| **Code emulation**     | Runs the code in a virtual CPU to watch what it tries to do          | **Watches actions** made by programs and can stop suspicious ones     |

**📌 Trap:** Signature-based detection only catches **known** malware. To catch **new/unknown/polymorphic** malware you need **heuristic/behavioral** detection.

## 12. Ports Malware Loves (quick recall)

| **Port** | **Protocol / Service** | **Why malware uses it** | **Beginner explanation**                                                              | **Easy memory** |
|------------------------------------|--------------------------------------------------|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| **6667**                           | **IRC**                                          | **Classic botnet C2 channel**                     | **Malware can use IRC to receive commands from the attacker**                                                   | **6667 → IRC → Botnet**                   |
| **80**                             | **HTTP**                                         | **Communicates through normal web traffic**       | **Malware can communicate with its attacker using normal-looking web traffic**                                  | **80 → HTTP → Web**                       |
| **443**                            | **HTTPS**                                        | **Encrypted web communication**                   | **Malware can communicate through encrypted web traffic, making the communication harder to inspect**           | **443 → HTTPS → Secure Web**              |
| **445**                            | **SMB**                                          | **Network spreading / file sharing**              | **Malware can use SMB to move between Windows computers when vulnerabilities or weak access controls allow it** | **445 → SMB → Spread**                    |

## 13. Tools (tool → purpose)

| **Tool**     | **Purpose**  | **Simple explanation** | **Analysis Type** |
|----------------------------------------|----------------------------------------|--------------------------------------------------|---------------------------------------------|
| **Cuckoo Sandbox**                     | **Automated dynamic malware analysis** | **Runs malware safely and watches what it does** | **🟢 Dynamic**                              |
| **VirusTotal / Hybrid Analysis**       | **Online multi-engine scanning**       | **Checks a file with many security engines**     | **🔵 Both / depends on service**            |
| **PEiD / Detect It Easy (DIE)**        | **Detect packers/obfuscation**         | **Finds if malware is packed or hidden**         | **🔵 Static**                               |
| **Strings / FLOSS / BinText**          | **Extract embedded text**              | **Finds readable text inside a file**            | **🔵 Static**                               |
| **IDA / Ghidra / OllyDbg**             | **Disassembly / reverse engineering**  | **Looks at the program's code**                  | **🔵 Static**                               |
| **Process Monitor / Process Explorer** | **Watch process & system activity**    | **Shows what programs do while running**         | **🟢 Dynamic**                              |
| **Wireshark / TCPView**                | **Watch network connections for C2**   | **Shows network communication**                  | **🟢 Dynamic**                              |
| **njRAT**                              | **Example RAT**                        | **Remote-access malware**                        | **❌ Neither**                              |

## 14. Exam Traps (read twice) ⚠️

- **Virus needs user action; worm spreads by itself.** (The single most common malware question.)

- **Polymorphic** = changes appearance (encrypts). **Metamorphic** = rewrites its whole code.

- **Dropper** carries the malware inside it; **downloader** fetches it from the internet.

- **Trojan** = disguised as legit; runs at the user’s privilege level.

- **Fileless** = memory-only, abuses PowerShell/WMI (Living off the Land).

- **APT** = long-term, stealthy, targeted (usually nation-state).

- **Static** = don’t run it. **Dynamic** = run it in a sandbox.

- **Signature** detection misses unknown malware; **heuristic** can catch it.

- Ransomware best defense = **offline backups**; WannaCry used EternalBlue (SMB 445).

## 15. Mnemonics

**Virus vs Worm:** “Virus = Victim clicks. Worm = Wanders on its own.”

**Poly vs Meta:** “Poly Paints over (same code, new look). Meta Makes it new (rewrites code).”

**Static vs Dynamic:** “Static = Study it still. Dynamic = Detonate it.”

**Dropper vs Downloader:** “Dropper Delivers what it Carries; Downloader Dials out to fetch.”

## 16. What to Memorize Cold 🎯

- The malware types table (Virus, Worm, Trojan, Ransomware, Rootkit, Spyware, Keylogger, Botnet, Fileless, APT).

- Virus needs user action; worm self-spreads.

- Polymorphic vs Metamorphic; Dropper vs Downloader.

- Malware components: Payload, Dropper, Downloader, Injector, Crypter, Packer, Obfuscator.

- Trojan types: RAT, Backdoor, Botnet, Banking, POS, Command Shell, HTTP/HTTPS.

- Fileless = memory-only + LOL (PowerShell/WMI); APT = long-term targeted.

- Static vs Dynamic analysis (and Sheep Dip, Sandbox, Cuckoo, VirusTotal).

- Detection: signature vs heuristic; heuristic catches unknown malware.

- Ports: 6667 IRC C2, 80/443 web C2, 445 SMB (EternalBlue).

**Exam behavior tip:** Malware questions are mostly “match the behavior to the type.” Read for the **key behavior** (spreads by itself? disguised? encrypts files? memory-only? rewrites code?) and map it to the exact malware name.

 **E-banking trojan** attacks the **victim's own computer** during **online banking** (fake form fields, TAN grabbers, form grabbers). Example: CHAVECLOAK.

 **POS trojan** attacks the **shop's payment terminal** during **in-store card payment**.

"**Living off the land**" means using **tools already installed on the system** (PowerShell, WMI, scheduled tasks) instead of bringing custom tools. It's one of the main reasons attackers use fileless malware, because trusted tools don't look suspicious. (Source: Project documents, *07 - Malware threats.md*)

**Why the others are wrong**

- **A. Sheep dipping** is a **defense**: checking suspicious files on an isolated computer before they enter the network.

- **C. Code instrumentation** is an **analysis** technique: inserting extra code into a binary to study it (e.g., HawkEye).

- **D. Reflective DLL injection** loads a malicious DLL directly into a process's memory. It's a fileless technique, but the clue here is "built-in trusted tools", which points to LOL.

<!-- -->

- **Sheep dipping** = a **checkpoint**: scan incoming files/USBs before they enter the network.

- **Dynamic analysis** = **study** the malware by running it and observing behavior.

<!-- -->

- **A. Static analysis** examines the file **without running it** (strings, hashes, headers, disassembly).

- **D. Code emulation** is an **antivirus detection method**, not an analyst's investigation.
