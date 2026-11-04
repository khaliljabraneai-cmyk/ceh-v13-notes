**Module 03 — Scanning Networks**

If Module 02 was the burglar building a dossier on the house from public records and a bit of doorbell-testing, Module 03 is when they walk the entire perimeter of the house checking every single door and window — systematically, methodically, looking for what's actually open. This is **Phase 2 of the 5 Hacking Phases**, and per your Master Study Guide it's part of the highest-weighted domain on the exam: "Network & Perimeter Hacking" at 24% (shared with Modules 9-12).

# **1. What is Scanning? 🏠**

Per your Walker guide: in the real world, scanning just means making "a big list of what's live on the network." EC-Council expands this to also include discovering the OS, architecture, running services — and even a first look at vulnerabilities.

**Goal:** Take the broad target picture from Footprinting and narrow it down to: which hosts are alive, which ports are open, what services are running, and what OS is in use.

# **1a. The 3 Officially Named Scanning Techniques (per EC-Council courseware — supplementary confirmation)**

Cross-checking against supplementary CEH v13 course notes, EC-Council groups scanning into three official types (these match the “Types of Scanning” in section 3): Port Scanning, Network Scanning, and Vulnerability Scanning:

| **Technique**              | **What It Does**                                                                                                                                                                                                 |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Port Scanning**          | Checking services running on the target by sending sequences of probe messages to identify open ports                                                                                                            |
| **Vulnerability Scanning** | Checking whether a system is exploitable by identifying known vulnerabilities in discovered services                                                                                                             |
| **Network Scanning**       | Identifying live hosts / active IP addresses on the network (ping sweeps, host discovery). NOTE: banner grabbing and OS fingerprinting are TECHNIQUES used during scanning, not one of the three official types. |

***Exam tip:** if a question asks you to name the three scanning types, the answer is Port, Network, and Vulnerability scanning. Banner grabbing / OS fingerprinting is a technique used WITHIN scanning — it is a common wrong option, so don’t pick it as one of the three types.*

# **2. Scanning vs. Footprinting vs. Enumeration (the sequence, clarified)**

| **Phase**                  | **Core Question It Answers**                               | **Scope**                                                                     |
|----------------------------|------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Footprinting (Phase 1)** | "What does this organization look like from the outside?"  | Broad, **mostly passive**                           |
| **Scanning (Phase 2)**     | "Which hosts/ports/services are actually live?"            | **Active**, typically extranet-focused (per Walker) |
| **Enumeration (Phase 3)**  | "What detailed info can I extract by actively connecting?" | **Active**, typically intranet-focused (per Walker) |

**Exam trap reminder:** Vulnerability discovery is officially Phase 4, but EC-Council's broadened Scanning definition does touch on it — don't assume "vulnerability" mentioned in a question always means Phase 4.

# **3. Types of Scanning**

| **Type**                   | **House Analogy**                                           | **Purpose**                                              |
|----------------------------|-------------------------------------------------------------|----------------------------------------------------------|
| **Port Scanning**          | Checking every door and window to see which are unlocked    | Identify open ports/services on a host                   |
| **Network Scanning**       | Walking the whole street to see which houses have lights on | Identify live hosts on a network                         |
| **Vulnerability Scanning** | Checking if the unlocked window's latch is broken too       | Identify known weaknesses in discovered systems/services |

# **4. TCP Three-Way Handshake (foundational — you MUST know this cold)**

Before understanding scan types, you need the handshake:

**SYN → SYN-ACK → ACK**

| **Step** | **Direction**   | **Flag** |
|----------|-----------------|----------|
| **1**    | Client → Server | SYN      |
| **2**    | Server → Client | SYN-ACK  |
| **3**    | Client → Server | ACK      |

This matters because every scan type below is defined by how it manipulates this handshake.

# **5. Critical Nmap Scan Types (HIGH exam priority)**

| **Flag** | **Scan Type**     | **How It Works**                                                        | **Notes**                                                          |
|----------|-------------------|-------------------------------------------------------------------------|--------------------------------------------------------------------|
| **-sS**  | SYN/Stealth Scan  | Sends SYN, gets SYN-ACK, then sends RST instead of completing handshake | Most common, "half-open," doesn't complete connection = stealthier |
| **-sT**  | TCP Connect Scan  | Completes the full 3-way handshake                                      | Noisier, logged easily, doesn't need root privileges               |
| **-sU**  | UDP Scan          | Sends UDP packets, checks for ICMP "port unreachable" replies           | Finds UDP services; notoriously slow                               |
| **-sF**  | FIN Scan          | Sends only a FIN flag                                                   | Can bypass simple firewalls/ACLs that only watch for SYN           |
| **-sX**  | Xmas Scan         | Sends FIN + PSH + URG flags together ("lit up like a Christmas tree")   | Closed ports reply RST; open/filtered ports often stay silent      |
| **-sN**  | NULL Scan         | Sends a packet with NO flags set                                        | Same closed/open logic as Xmas scan                                |
| **-sA**  | ACK Scan          | Sends only an ACK flag                                                  | Doesn't determine open/closed — used to map firewall rules         |
| **-O**   | OS Detection      | Analyzes response patterns                                              | Fingerprints the target's operating system                         |
| **-sV**  | Version Detection | Probes open ports further                                               | Identifies exact service/version running                           |
| **-A**   | Aggressive Scan   | Combines OS detection + version detection + script scanning             | All-in-one, but very noisy/detectable                              |

**Easy memory trick for FIN/Xmas/NULL:** These three are all "sneaky" scans that avoid a full handshake. If the port is closed, the target sends back an RST. If it's open or filtered, there's typically no response at all ("silence = maybe open").

**5b. Idle / Zombie Scan and Host Discovery (added — high exam value) 🥷**

The scan table above is missing two things the exam tests a lot: the **idle/zombie scan** (the stealthiest scan of all) and the **host-discovery / ping flags**.

**Idle (Zombie) Scan — the “frame someone else” scan.** You bounce your probes off a third, idle computer (the “zombie”). The target only ever sees the zombie’s IP, never yours. It uses the zombie’s IP-ID numbers to work out which ports are open. Result: your real address never appears in the target’s logs — fully spoofed source.

| **Flag** | **Scan**           | **Exam point**                                            |
|----------|--------------------|-----------------------------------------------------------|
| -sI      | Idle / Zombie scan | Stealthiest; source IP is fully spoofed via a zombie host |

**Host discovery (ping) flags — “who is alive before I scan ports?”**

| **Purpose**                                 | **Flag** |
|---------------------------------------------|----------|
| No port scan, just find live hosts          | -sn      |
| ARP ping (local network only)               | -PR      |
| TCP SYN ping                                | -PS      |
| TCP ACK ping (can slip past some firewalls) | -PA      |
| ICMP echo ping                              | -PE      |
| UDP ping                                    | -PU      |

**🔥 Exam takeaway:** if a question wants the scan where the attacker’s IP never shows in the logs → idle/zombie scan (-sI). If it just wants “fast + stealthy from my own IP” → SYN scan (-sS).

**5c. IDS / Firewall Evasion (added — high exam value) 🧱**

Nmap can also try to slip past firewalls and intrusion-detection systems. Know these options and what they do:

| **Technique**     | **Flag**           | **What it does**                                             |
|-------------------|--------------------|--------------------------------------------------------------|
| Fragmentation     | -f                 | Splits packets into tiny fragments to dodge signature IDS    |
| Decoys            | -D                 | Adds fake source IPs so the real one is hidden in the crowd  |
| Timing control    | -T0 to -T5         | T0 = paranoid/slowest (stealth), T5 = insane/fastest (noisy) |
| Source-port spoof | -g / --source-port | Sends from a trusted port like 53 (DNS) or 80                |
| Idle/zombie       | -sI                | Hides behind a third host (see 5b)                           |

**Timing templates:** T0 Paranoid, T1 Sneaky, T2 Polite, T3 Normal (default), T4 Aggressive, T5 Insane. Lower = stealthier and slower; higher = faster and louder.

**TTL hint (bonus):** default TTL can reveal the OS — Linux ≈ 64, Windows ≈ 128, network devices ≈ 255.

# **6. Port States (what your scan results actually mean)**

| **State**      | **Meaning**                                                                            |
|----------------|----------------------------------------------------------------------------------------|
| **Open**       | A service is actively listening and responding                                         |
| **Closed**     | Port is reachable but no service is listening                                          |
| **Filtered**   | A firewall/filter is blocking Nmap from determining the state                          |
| **Unfiltered** | Port is accessible, but Nmap can't tell if it's open or closed (common with ACK scans) |

# **7. Other Key Scanning Tools**

| **Tool**             | **Purpose**                                                     |
|----------------------|-----------------------------------------------------------------|
| **Nmap**             | The core scanning tool — port scanning, OS detection, scripting |
| **Zenmap**           | GUI front-end for Nmap                                          |
| **Hping3**           | Custom packet crafting — great for firewall/IDS evasion testing |
| **Masscan**          | Extremely fast port scanner for large IP ranges                 |
| **Angry IP Scanner** | Simple, fast IP/port scanner, cross-platform                    |

# **8. Common Ports Worth Memorizing**

| **Port**  | **Protocol** | **Remember As** |
|-----------|--------------|-----------------|
| **20/21** | FTP          | File Transfer   |
| **22**    | SSH          | Secure Shell    |
| **23**    | Telnet       | Insecure Remote |
| **25**    | SMTP         | Send Email      |
| **53**    | DNS          | Domain Lookup   |
| **80**    | HTTP         | Web Traffic     |
| **110**   | POP3         | Receive Email   |
| **143**   | IMAP         | Email Sync      |
| **443**   | HTTPS        | Secure Web      |
| **445**   | SMB          | Windows Shares  |
| **3306**  | MySQL        | Database        |
| **3389**  | RDP          | Remote Desktop  |

**Exam tip:** At least 10-15 real exam questions typically involve port numbers in some form — either directly ("which port does X use?") or indirectly in a scenario ("traffic on port 3389 was detected...").

# **9. Banner Grabbing**

A technique for identifying the exact software/version running on an open port by capturing the "banner" (welcome message) a service returns when connected to.

- **Active Banner Grabbing** — directly connecting via telnet/netcat to a port to read the response

- **Passive Banner Grabbing** — using a third-party service (like Shodan) that already collected the banner, avoiding direct contact

**The story: You're checking houses on a street**

Imagine you're walking down a street, and every house is a "port" (a door) on a computer. You want to know: **is someone home, and what are they doing?**

**Step 1: The handshake (how a *normal* visit works)**

If you want to visit someone properly, there's a 3-step ritual:

1.  You knock: **"Hello?"** (this is called SYN)

2.  They open the door: **"Hi, come on in!"** (this is called SYN-ACK)

3.  You say: **"Thanks, I'm coming in"** and you actually walk inside (this is called ACK)

That's it. Knock → Invited in → Walk in. Three steps = a real visit.

**Why this matters:** Every scan trick below is just a different way of playing with these 3 steps — either skipping a step, or doing something weird instead.

**Step 2: The scan types (different ways to check houses without a real visit)**

Let's go one at a time, slowly.

**-sS (SYN Scan) — "the knock and run"**

- You knock.

- They say "come in!"

- But instead of walking in, you just walk away.

- Why? Because you already got your answer (someone's home) without needing to actually go inside. It's fast and quiet — nobody remembers a person who just knocked and left.

**-sT (Connect Scan) — "the full visit"**

- You knock, they invite you in, and you actually walk inside and sit down.

- This is a *complete* visit — the homeowner will definitely remember you and write it in their guestbook (a log file). Noisier, easier to catch.

**-sU (UDP Scan) — "shouting through a mail slot"**

- No knocking ritual here at all. You just shout a message through the mail slot.

- If nobody replies at all, you assume someone might be home (or they're just ignoring you).

- If you get an error message back ("this house doesn't exist"), you know it's empty.

- This is slow and confusing because silence doesn't always mean what you think.

**-sF, -sX, -sN (FIN, Xmas, NULL) — "the weird notes"**  
These three are all cousins. Instead of a normal knock, you slide something strange under the door:

- **FIN**: a note that says "goodbye" (without ever saying hello first — weird, right?)

- **Xmas**: a note with THREE weird symbols scribbled on it (called "lit up like a Christmas tree")

- **NULL**: a completely blank note

Why do this? Because some old, simple security systems only watch for a normal "knock" (SYN). They don't know how to react to a weird blank note or a random "goodbye" — so you can sneak past them.

What happens when you slide these weird notes:

- **House is empty (port closed)** → someone yells back "wrong house!" (this is called RST)

- **Someone's home (port open)** → total silence, because your weird note confused them and they don't know how to respond

So: **silence = maybe someone's home. A "wrong house" reply = definitely empty.**

**-sA (ACK Scan) — "checking if the door is even locked"**

- You're not trying to see if someone's home at all.

- You're just checking: does this house *have* a working door, or is there a big fence (firewall) blocking you entirely?

- This tells you about security setup, not about people being home.

**-O (OS Detection) — "guessing the house style"**

- By looking at how the house is built (how it reacts to your knocks), you guess: "this looks like a brick house" (Windows) or "this looks like a wooden cabin" (Linux).

**-sV (Version Detection) — "asking for more details"**

- Once you know someone's home, you ask them directly: "What's your name? How old are you?"

- This tells you exactly what software they're running and which version.

**-A (Aggressive Scan) — "doing everything at once"**

- Knock, guess the house style, ask for details, AND try extra scripts — all in one go.

- Fast and thorough, but very obvious/loud. Everyone on the street will notice you.

**Step 3: Port states (what you actually learn about each house)**

| **What you find** | **What it means**                                                    |
|---------------------------------------------|------------------------------------------------------------------------------------------------|
| **Open**          | Someone answers the door — a service is running                      |
| **Closed**        | Nobody's home, but the house clearly exists (you got a clear answer) |
| **Filtered**      | There's a wall/guard and you can't even tell if anyone lives there   |
| **Unfiltered**    | You can reach the door, but still can't tell open vs closed          |

**Step 4: The tools (different gadgets for checking houses)**

- **Nmap** — your main toolkit, does everything above

- **Zenmap** — same as Nmap, but with buttons and pictures instead of typed commands

- **Hping3** — for building your own custom "weird notes" to test tricky security systems

- **Masscan** — same idea as Nmap, but built to check thousands of streets (IP addresses) super fast

- **Angry IP Scanner** — a simple, beginner-friendly version

**Step 5: Common ports (just "known house numbers" for certain services)**

Don't overthink this — just match the number to the job:

- **20/21** = FTP (sending files)

- **22** = SSH (secure remote login)

- **23** = Telnet (insecure remote login — old and risky)

- **25** = SMTP (sending email)

- **53** = DNS (looking up website addresses)

- **80** = HTTP (regular websites)

- **110** = POP3 (receiving email)

- **143** = IMAP (syncing email)

- **443** = HTTPS (secure websites)

- **445** = SMB (Windows file sharing)

- **3306** = MySQL (databases)

- **3389** = RDP (remote desktop control)

**Step 6: Banner grabbing ("reading the welcome sign")**

When a house door opens, sometimes there's a **sign hanging there** that says exactly who lives there: "Hi, I'm Apache, version 2.4."

- **Active banner grabbing** = you personally knock and read the sign yourself

- **Passive banner grabbing** = someone else already knocked earlier and wrote down what the sign said (like Shodan) — you just read their notes, without ever knocking yourself

# **10. Scanning Countermeasures (defender's side)**

- Configure firewalls to drop/reject unsolicited scan packets rather than respond predictably

- Use IDS/IPS to detect scan patterns (many connection attempts in a short time)

- Disable banner information or replace with generic responses

- Regularly audit which ports are actually needed to be open

# **11. V13 AI Addition 🤖**

While your Master Study Guide's dedicated V13 AI table maps most AI content to Modules 2, 5, 7, 9, and 12, Scanning benefits from the same automation theme: AI-assisted scanning tools can prioritize which hosts/ports are most likely vulnerable, speeding up what used to be manual triage across scan results — same "force-multiplier" theme as Module 02's AI addition, just applied to scan result analysis instead of OSINT gathering.

# **🎯 WHAT TO MEMORIZE COLD FOR THE EXAM (Module 03)**

1.  **The TCP 3-way handshake** (SYN → SYN-ACK → ACK) — foundation for every scan type

2.  **All major Nmap flags and what they do**: -sS, -sT, -sU, -sF, -sX, -sN, -sA, -sI (idle/zombie), -O, -sV, -A, plus host discovery -sn/-PR and evasion -f, -D, -T0–T5

3.  **Port states**: Open, Closed, Filtered, Unfiltered — and what causes each

4.  **FIN/Xmas/NULL scan logic**: closed port = RST reply; open/filtered = silence

5.  **Scanning vs. Enumeration distinction**: extranet vs. intranet (Walker's exam tip)

6.  **Common ports** — at minimum: 20/21, 22, 23, 25, 53, 80, 110, 143, 443, 445, 3306, 3389

7.  **Active vs. Passive banner grabbing**

8.  **The 3 officially named scanning techniques**: Port Scanning, Vulnerability Scanning, Banner Grabbing

**Exam behavior tip specific to this module:** Expect direct "what does this Nmap flag do" questions AND scenario questions describing packet behavior (e.g., "a scan sends FIN+PSH+URG and gets no response from several ports") asking you to name the scan type and correctly determine the port's likely state.

Every TCP packet has a little header with several "flag" bits that can be turned on or off. Think of them as **checkboxes on the note you slide under the door** — each one tells the receiving computer something different about what kind of message this is. You've already met one: **SYN** ("hello, let's start") and **ACK** ("okay, confirmed"). Here are the other three:

**FIN — "Finish" (I'm done talking)**

- Normally used to **politely end** a connection that was already open.

- It's like saying "okay, goodbye, closing the door now."

- The weird part in a FIN scan: you send this "goodbye" **without ever having said hello first**. That's not normal behavior, which is exactly why it can confuse simple firewalls.

**PSH — "Push" (don't wait, deliver this now)**

- Normally used to tell the receiving computer: "don't hold this data in a buffer waiting for more — hand it over to the application immediately."

- Think of it like writing **"URGENT — READ NOW"** on an envelope instead of leaving it in the mailbox pile.

**URG — "Urgent" (this part of the data is high priority)**

- Normally marks that **part of the data** inside the packet is urgent and should be processed out of the normal order.

- Like circling one sentence in a letter and writing **"skip to this line first."**

**Why Xmas scan uses these three together**

Under **normal, correct communication**, you'd never see FIN + PSH + URG all lit up together with no prior handshake. It's a meaningless, nonsensical combination — nobody's actually trying to "finish, push, and mark urgent" all at once out of nowhere.

That's the whole point: it's **not meant to make logical sense**. It's meant to be a weird, malformed packet that:

- Confuses simple/old systems that don't handle unexpected flag combos well

- Still respects the official TCP rules well enough that *some* systems will respond in a predictable way (RST if closed, silence if open) — which is what lets Nmap infer something from it

So really, don't think of these three flags as doing their "official job" here. In a Xmas scan, they're being **misused on purpose**, purely to see how the target reacts to something abnormal.

FIN/Xmas/NULL scans are unreliable against Windows because Windows does not follow the expected RFC behavior for these abnormal TCP packets.

**CVE = identifies a vulnerability.**  
**MITRE ATT&CK = describes attacker techniques/tactics.**

- **"What does this vulnerability code mean?"** That's a **CVE database** question.

- **"What ATT&CK techniques/tactics could an attacker use with this vulnerability?"** that's a **MITRE ATT&CK** question.

<https://attack.mitre.org/>

<https://www.cve.org/>

**Scanning = “What is alive?”** 🌐  
**Enumeration = “Tell me more about it.”** 🔍

**Scanning is typically extranet-focused; Enumeration is typically intranet-focused.**
