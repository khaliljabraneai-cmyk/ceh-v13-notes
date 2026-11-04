**CEH v13 — Exam-Focused Summary (v3 style)**

# Module 06 — System Hacking

If Vulnerability Analysis (Module 05) was the home inspector listing every weak lock and rotten window, **System Hacking is where the burglar actually breaks in, grabs the master keys, hides a spare key for next time, and wipes their fingerprints on the way out**. This is the heart of the attack. Per your Master Study Guide it sits in the **System Hacking Phases & Attack Techniques** domain (~15% of the exam) — one of the most heavily tested modules.

*Source: CEH v13 Master Study Guide (Drive) + Chapter 7 Summary (Drive) + Mr-Infect 06 - System hacking notes*

## 1. What is System Hacking? 🏠

System hacking is the process of **gaining access to a target system, raising your privileges, keeping that access, and then hiding the evidence**. It comes right after Enumeration and Vulnerability Analysis in the CEH methodology — you already know the weaknesses; now you use them.

**⚠️ Two “phase” lists again:** In the **5 Phases of Hacking**, System Hacking covers phases 3–5 (Gaining Access → Maintaining Access → Covering Tracks). In the **CEH Hacking Methodology**, System Hacking is the final step (after Vulnerability Analysis). Same activity, two maps.

**5 Phases = WHAT the attacker does /**

**5 Phases: Recon → Scan → Gain Access → Maintain Access → Cover Tracks**

**CEH Methodology = HOW the CEH process is organized**

**CEH Methodology:**  
**Footprinting → Scanning → Enumeration → Vulnerability Analysis → System Hacking → …**

| **Framework**               | **Where is System Hacking?**                                                   |
|-----------------------------|--------------------------------------------------------------------------------|
| **5 Phases of Hacking**     | Covers **Phases 3 → 5**: Gaining Access → Maintaining Access → Covering Tracks |
| **CEH Hacking Methodology** | **A final step/activity**, performed **after Vulnerability Analysis**          |

## 2. The 4 Sub-Phases of System Hacking (memorize the order) 🎯

Think of the burglar’s job once they’re at the door:

| **\#** | **Sub-phase**             | **House analogy**                | **What the attacker does**                                     |
|--------|---------------------------|----------------------------------|----------------------------------------------------------------|
| 1      | **Gaining Access**        | Break in through the weak window | Exploit a vulnerability or crack a password to get in          |
| 2      | **Escalating Privileges** | Find the master key              | Move from a normal user to Admin / root                        |
| 3      | **Maintaining Access**    | Hide a spare key                 | Install backdoors, rootkits, or rogue accounts to return later |
| 4      | **Covering Tracks**       | Wipe fingerprints                | Clear logs, hide files, defeat forensics                       |

**🧠 Mnemonic — “GEMC”:** Gaining access → Escalating privileges → Maintaining access → Covering tracks.

## 3. How Windows Stores Passwords (exam favorite)

Windows keeps password **hashes** (one-way scrambled values, not the real password) in the **SAM** file (Security Accounts Manager). On a domain, Active Directory stores them in **ntds.dit**.

| **Item**     | **Detail**                                                                   |
|--------------|------------------------------------------------------------------------------|
| SAM location | %SystemRoot%\\system32\\config\\SAM                                          |
| Protected by | SYSKEY partially encrypts the hashes; SAM is locked while Windows is running |
| Hash types   | LM (old, weak, disabled since Vista) and NTLM / NTLMv2 (default)             |
| Domain store | ntds.dit on the Domain Controller                                            |

**📌 Exam point:** SAM = local accounts. ntds.dit = domain (AD) accounts. LM is the weak/legacy hash; NTLMv2 is stronger but still weaker than Kerberos.

**SAM secures the Single machine; AD manages the whole Domain**

*Source: Mr-Infect 06 notes + Chapter 7 Summary (Drive)*

## 4. NTLM vs Kerberos Authentication

|           | **NTLM**               | **Kerberos**                                                    |
|-----------|------------------------|-----------------------------------------------------------------|
| Style     | Challenge/response     | Ticket-based (secret-key crypto)                                |
| Key parts | Client, Server, SAM/AD | KDC = AS (Authentication Server) + TGS (Ticket Granting Server) |
| Strength  | Weaker (NTLMv2 = ok)   | Stronger — the modern default                                   |
| Exam cue  | “challenge/response”   | “TGT / service ticket / KDC”                                    |

**Kerberos flow (simple):** Log in → get a **TGT** (Ticket-Granting Ticket) → ask TGS for a **service ticket** → use it to access the service.

## 5. Password Attack Categories (4 groups — high yield)

CEH splits **how** you attack a password into 4 categories. Know which category an attack belongs to.

| **Type**              | **Technique**                  | **Simple phrase to remember**                       |
|-----------------------|--------------------------------|-----------------------------------------------------|
| **🟢 Non-electrical** | Shoulder Surfing               | 👀 Watch someone type their password                |
| **🟢 Non-electrical** | Dumpster Diving                | 🗑️ Search the trash for useful information          |
| **🟢 Non-electrical** | Social Engineering             | 🎭 Trick a person into giving information           |
| **🔵 Active Online**  | Password Guessing              | 🔑 Try passwords you think they might use           |
| **🔵 Active Online**  | Dictionary Attack              | 📖 Try passwords from a word list                   |
| **🔵 Active Online**  | Brute Force                    | 🔨 Try every possible combination                   |
| **🔵 Active Online**  | Password Spraying              | 💦 One common password → many accounts              |
| **🔵 Active Online**  | Credential Stuffing            | ♻️ Stolen credentials → try them elsewhere          |
| **🔵 Active Online**  | Hash Injection / Pass-the-Hash | 💉 Use a stolen hash instead of the password        |
| **🔵 Active Online**  | LLMNR/NBT-NS Poisoning         | 🎣 Give a fake answer → capture authentication data |
| **🔵 Active Online**  | Keylogger                      | ⌨️ Record what the victim types                     |
| **🟡 Passive Online** | Wire Sniffing                  | 👂 Listen to network traffic                        |
| **🔵 Active Online**  | MITM                           | 🕴️ Intercept communication between two parties      |
| **🔵 Active Online**  | Replay Attack                  | 🔁 Capture authentication data → reuse it           |
| **🔴 Offline**        | Rainbow Table                  | 🌈 Use precomputed hashes to find the password      |
| **🔴 Offline**        | Stolen Hash Cracking           | 💻 Take stolen hashes → crack them offline          |

**🧠 Mnemonic — “NAPO”:** Non-electronic, Active online, Passive online, Offline.

## 6. Password Cracking Techniques (know the difference)

| **Technique** | **Simple meaning**                             | **🧠 Remember**                                  |
|---------------|------------------------------------------------|--------------------------------------------------|
| Dictionary    | Try words from a wordlist                      | 📖 Try the labeled keys                          |
| Brute force   | Try every possible combination                 | 🔒 Try every combo on the padlock                |
| Hybrid        | Dictionary words + small changes (add123, C@p) | 🔧 Common keys, then tweak them                  |
| Rule-based    | Apply transformation rules to guesses          | When you know something about the password       |
| Rainbow table | Look up precomputed hash → password            | 📕 Pre-made fingerprint lookup book              |
| Mask attack   | Brute force following a known pattern          | hashcat with a pattern (e.g. 8 chars, 1 capital) |

**📌 Exam trap — Rainbow tables:** the defense is **salting** (adding a random string before hashing). Salt makes precomputed rainbow tables useless.

## 7. Credential Attacks You MUST Recognize (very testable)

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 28%" />
<col style="width: 41%" />
<col style="width: 13%" />
</colgroup>
<thead>
<tr class="header">
<th><h3 id="attack" class="unnumbered"><mark>Attack</mark></h3></th>
<th><h3 id="what-it-does" class="unnumbered"><mark>What it does</mark></h3></th>
<th><h3 id="simple-idea-explanation" class="unnumbered"><mark>Simple idea / Explanation</mark></h3></th>
<th><h3 id="tool" class="unnumbered"><mark>Tool</mark></h3></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><h2 id="pass-the-hash-pth" class="unnumbered">Pass-the-Hash (PtH)</h2></td>
<td><h2 id="reuse-a-stolen-ntlm-hash-to-authenticate-no-password-needed" class="unnumbered">Reuse a stolen NTLM hash to authenticate — no password needed</h2></td>
<td><h2 id="use-the-stolen-hash-as-a-password-substitute.-the-attacker-logs-in-without-knowing-the-real-password." class="unnumbered">Use the stolen hash as a password substitute. The attacker logs in without knowing the real password.</h2></td>
<td><h2 id="mimikatz" class="unnumbered">Mimikatz</h2></td>
</tr>
<tr class="even">
<td><h2 id="pass-the-ticket" class="unnumbered">Pass-the-Ticket</h2></td>
<td><h2 id="reuse-a-stolen-kerberos-ticket-instead-of-a-password" class="unnumbered">Reuse a stolen Kerberos ticket instead of a password</h2></td>
<td><h2 id="use-a-stolen-kerberos-ticket-as-an-access-pass.-no-password-is-needed." class="unnumbered">Use a stolen Kerberos ticket as an access pass. No password is needed.</h2></td>
<td><h2 id="mimikatz-1" class="unnumbered">Mimikatz</h2></td>
</tr>
<tr class="odd">
<td><h2 id="kerberoasting" class="unnumbered">Kerberoasting</h2></td>
<td><h2 id="crack-service-account-hashes-tgs-offline" class="unnumbered">Crack service-account hashes (TGS) offline</h2></td>
<td><h2 id="get-a-service-accounts-tgs-related-hash-material-and-crack-it-offline-to-discover-the-password." class="unnumbered">Get a service account's TGS-related hash material and crack it offline to discover the password.</h2></td>
<td><h2 id="rubeus-hashcat" class="unnumbered">Rubeus, Hashcat</h2></td>
</tr>
<tr class="even">
<td><h2 id="as-rep-roasting" class="unnumbered">AS-REP Roasting</h2></td>
<td><h2 id="attack-accounts-with-kerberos-pre-auth-disabled" class="unnumbered">Attack accounts with Kerberos pre-auth disabled</h2></td>
<td><h2 id="find-accounts-with-pre-authentication-disabled-obtain-as-rep-data-then-crack-it-offline-to-recover-the-password." class="unnumbered">Find accounts with pre-authentication disabled, obtain AS-REP data, then crack it offline to recover the password.</h2></td>
<td><h2 id="rubeus" class="unnumbered">Rubeus</h2></td>
</tr>
<tr class="odd">
<td><h2 id="llmnr-nbt-ns-poisoning" class="unnumbered">LLMNR / NBT-NS Poisoning</h2></td>
<td><h2 id="spoof-name-resolution-replies-to-capture-hashes" class="unnumbered">Spoof name-resolution replies to capture hashes</h2></td>
<td><h2 id="pretend-to-be-the-server-the-victim-is-looking-for-making-the-victim-connect-to-you-and-potentially-send-authentication-data." class="unnumbered">Pretend to be the server the victim is looking for, making the victim connect to you and potentially send authentication data.</h2></td>
<td><h2 id="responder" class="unnumbered">Responder</h2></td>
</tr>
<tr class="even">
<td><h2 id="ntlm-relay" class="unnumbered">NTLM Relay</h2></td>
<td><h2 id="intercept-and-relay-an-ntlm-auth-request" class="unnumbered">Intercept and relay an NTLM auth request</h2></td>
<td><h2 id="forward-the-victims-authentication-to-another-server-instead-of-cracking-the-authentication." class="unnumbered">Forward the victim's authentication to another server instead of cracking the authentication.</h2></td>
<td><h2 id="responder-ntlmrelayx" class="unnumbered">Responder, ntlmrelayx</h2></td>
</tr>
<tr class="odd">
<td><h2 id="golden-ticket" class="unnumbered">Golden Ticket</h2></td>
<td><h2 id="forge-a-tgt-after-compromising-the-krbtgt-account-domain-wide-control" class="unnumbered">Forge a TGT after compromising the krbtgt account — domain-wide control</h2></td>
<td><h2 id="steal-the-krbtgt-secret-and-create-fake-tgts-giving-potentially-domain-wide-access." class="unnumbered">Steal the KRBTGT secret and create fake TGTs, giving potentially domain-wide access.</h2></td>
<td><h2 id="mimikatz-2" class="unnumbered">Mimikatz</h2></td>
</tr>
<tr class="even">
<td><h2 id="silver-ticket" class="unnumbered">Silver Ticket</h2></td>
<td><h2 id="forge-a-single-service-ticket-tgs" class="unnumbered">Forge a single service ticket (TGS)</h2></td>
<td><h2 id="create-a-fake-tgs-for-a-specific-service-giving-access-to-that-service-rather-than-the-whole-domain." class="unnumbered">Create a fake TGS for a specific service, giving access to that service rather than the whole domain.</h2></td>
<td><h2 id="mimikatz-3" class="unnumbered">Mimikatz</h2></td>
</tr>
</tbody>
</table>

## 8. Password Cracking Tools (tool → purpose)

| **Tool** | **Type / Purpose** | **Techniques it can perform** | **Simple idea**                                                   | **🧠 Remember** |
|------------------------------------|----------------------------------------------|---------------------------------------------------------|---------------------------------------------------------------------------------------------|-------------------------------------------|
| **Hashcat**                        | 🔓 Offline password/hash cracking            | **Dictionary, Brute force, Hybrid, Rule-based, Mask**   | You already have a hash → Hashcat tries different password guesses to recover the password. | 🚀 **Powerful offline cracker**           |
| **John the Ripper**                | 🔓 Offline password/hash cracking            | **Dictionary, Brute force, Hybrid, Rule-based**         | Takes a stolen hash and tries different password guesses.                                   | 🔨 **Classic hash cracker**               |
| **Ophcrack**                       | 🔓 Offline password cracking                 | **Rainbow table**                                       | Compares a stolen Windows hash against precomputed hash/password tables.                    | 📕 **Rainbow-table cracker**              |
| **Hydra (THC-Hydra)**              | 🌐 Online login brute-forcing                | **Dictionary, Brute force**                             | Tries many passwords directly against a **live login service**.                             | 🌐 **Attack the live login**              |
| **Medusa**                         | 🌐 Online login brute-forcing                | **Dictionary, Brute force**                             | Similar to Hydra; performs many login attempts against network services.                    | 🌐 **Hydra-like online brute-forcer**     |

## 9. Privilege Escalation

### Vertical vs Horizontal (classic exam pair)

| **Type**       | **Simple meaning**                                        | **Example**                            |
|----------------|-----------------------------------------------------------|----------------------------------------|
| **Vertical**   | Low user → higher privileges (up)                         | Normal user becomes Administrator/root |
| **Horizontal** | Same privilege level, but another user’s stuff (sideways) | User A accesses User B’s account/data  |

### Common techniques

| **Windows** | **Linux** | **Simple explanation**                                                                               | **🧠 Remember**    |
|---------------------------------------|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| **Unpatched kernel/service exploits** | **Kernel exploits**                 | Exploit a vulnerability in the OS kernel or a vulnerable service to gain higher privileges.                                    | 🐛 **Exploit a vulnerable system component** |
| **DLL hijacking**                     | —                                   | Trick a Windows program into loading a malicious DLL instead of the legitimate one.                                            | 📦 **Fake DLL gets loaded**                  |
| **Unquoted service paths**            | —                                   | Abuse a Windows service path that is not properly enclosed in quotes, potentially causing a malicious executable to be loaded. | 🚗 **Bad service path → wrong program**      |
| **UAC bypass**                        | —                                   | Find a way to execute something with elevated privileges without the normal UAC prompt.                                        | 🛡️ **Bypass UAC**                            |
| **Weak file permissions**             | **Weak file permissions**           | A privileged program/file can be modified by a lower-privileged user, allowing abuse to gain higher privileges.                | 📁 **Can modify what should be protected**   |
| **Token manipulation**                | —                                   | Abuse Windows access tokens to impersonate a more privileged user.                                                             | 🎟️ **Steal/use a powerful token**            |
| **Scheduled tasks**                   | **Cron jobs**                       | Abuse an automatically executed task/job that runs with higher privileges.                                                     | ⏰ **Wait for privileged execution**         |
| —                                     | **SUID/SGID binaries**              | Abuse a Linux executable that runs with the permissions of its owner/group rather than the current user.                       | 🔑 **Program runs with owner's privileges**  |
| —                                     | **Sudo misconfigurations**          | Abuse incorrectly configured sudo permissions to execute commands with higher privileges.                                      | 👑 **Allowed to run as root**                |
| —                                     | **NFS root-squash disabled**        | Abuse an NFS configuration that allows a remote root user to retain root privileges on exported files.                         | 🌐 **Remote root stays root**                |

**Tools:** Windows → PowerUp, BeRoot, Watson. Linux → LinPEAS, LinEnum, pspy. Multi → Metasploit / PEASS-ng.

**📌 Exam cues:** “unquoted service path” and “DLL hijacking” → Windows privesc. “SUID binary” and “sudo misconfig” → Linux privesc. “up a level” = vertical; “sideways to another user” = horizontal.

*Source: Chapter 7 Summary (Drive) + Mr-Infect 06 notes*

## 10. Maintaining Access — Backdoors & Rootkits

**"I already compromised the system → I want to be able to come back later."**

**Typical techniques/tools include:**

| **Technique** | **Purpose**                                | **Think** |
|-----------------------------------------|----------------------------------------------------------------------|-------------------------------------|
| **Backdoor**                            | **Creates a hidden way to reconnect**                                | **🚪 Secret door**                  |
| **Web shell**                           | **Remote command access through a web server**                       | **🌐 Hidden web access**            |
| **Rogue account**                       | **Creates/abuses an account for future access**                      | **👤 Secret account**               |
| **Keylogger**                           | **Records keystrokes/credentials**                                   | **⌨️ Record keys**                  |
| **Spyware**                             | **Secretly monitors/collects information**                           | **👁️ Spy**                          |
| **Rootkit**                             | **Hides malicious presence and can help maintain privileged access** | **🥷 Hide**                         |

**🥷 Rootkit hierarchy**

**Your table is essentially correct. Think "the deeper it is, the harder it is to detect/remove."**

| **Rootkit** | **Hides at** | **🧠 Remember** |
|---------------------------------------|----------------------------------------|-------------------------------------------|
| **Hardware/Firmware**                 | **Device firmware**                    | **🔧 Inside hardware**                    |
| **Hypervisor-level**                  | **Hypervisor below the OS**            | **🖥️ Below OS**                           |
| **Bootloader / Bootkit**              | **Boot process**                       | **🚀 Before OS**                          |
| **Kernel-level**                      | **OS kernel**                          | **⚙️ Inside kernel**                      |
| **Library-level**                     | **System libraries**                   | **📚 Libraries**                          |
| **Application-level**                 | **Applications**                       | **📦 Inside app**                         |

**🧠 Easy order to memorize**

**From deepest → shallowest:**

**Hardware → Hypervisor → Bootloader → Kernel → Library → Application**

**Detect rootkits:** integrity checkers (Tripwire, AIDE), memory-dump analysis. Anti-rootkit tools: GMER, TDSSKiller, Stinger.

## 11. Hiding Data — NTFS ADS & Steganography Techniques used to hide information so it is difficult to notice.

## NTFS ADS → secret data is stored in an extra stream attached to the existing file. 

## Steganography → secret data is embedded inside the file's actual content.

| **Concept**       | **Simple meaning**                                                      | **Exam point**                                          |
|-------------------|-------------------------------------------------------------------------|---------------------------------------------------------|
| **NTFS ADS**      | Alternate Data Streams — hide data/code inside an existing file on NTFS | Invisible in normal Explorer view; used to hide malware |
| **Steganography** | Hide a secret message inside an ordinary file (image, audio, video)     | Cover file looks normal to the eye                      |

**Tools:** ADS → StreamArmor, ADS Scanner, AlternateStreamView. Steg → OpenStego, SNOW (text), DeepSound (audio).

## 12. Covering Tracks (clearing the evidence)

The final step: destroy the proof so defenders can’t see what happened.

| **Technique**           |                                                                                  | **What it does**                                   |
|-------------------------|----------------------------------------------------------------------------------|----------------------------------------------------|
| **OS**                  | **Where the logs live / clear command**                                          |                                                    |
| Clear / manipulate logs |                                                                                  | Delete or edit event logs                          |
| Disable auditing        |                                                                                  | Stop the system from recording actions             |
| Timestomping            |                                                                                  | Change file timestamps to fool forensics           |
| Clear command history   |                                                                                  | Wipe shell history (e.g. .bash_history)            |
| Anti-forensics          |                                                                                  | Secure-delete / wipe artifacts, encrypt, obfuscate |
| Windows                 | Event Viewer logs; wevtutil, clearev (Meterpreter), auditpol to disable auditing |                                                    |
| Linux                   | /var/log; clear log files, wipe ~/.bash_history                                  |                                                    |

**📌 Exam cue:** “change file timestamps to hide activity” → Timestomping. “clearev” → Metasploit/Meterpreter log clearing. “auditpol” → disable Windows auditing.

*Source: Mr-Infect 06 notes (Hiding evidence) + Chapter 7 Summary (Drive)*

## 13. Metasploit — Quick Recognition

## Metasploit: An open-source penetration testing framework used to find, exploit, and test vulnerabilities in systems.

## ✅ The same Metasploit framework can be used by both.

## 👨‍💻 Ethical hacker (White Hat): uses Metasploit with authorization to test and improve security. 

## 😈 Black Hat: may use Metasploit without authorization to attack real systems. 

## 🧠 CEH exam tip: Metasploit itself is a legitimate penetration-testing tool. What makes its use ethical or malicious is the user's authorization and intent.

## 

| **Module** | **Purpose**                                                      |
|------------|------------------------------------------------------------------|
| Exploit    | Delivers the attack against a vulnerability                      |
| Payload    | The code that runs after a successful exploit (e.g. Meterpreter) |
| Auxiliary  | One-off actions: scanning, fuzzing, DoS                          |
| Encoder    | Obfuscates the payload to dodge AV/IDS                           |
| NOPS       | Generates NOP sleds (buffer padding)                             |

**Payload sub-types:** Singles (standalone), Stagers (set up the connection), Stages (downloaded by the stager).

| **Term**      | **Simple meaning**                               | **🧠 Remember**   |
|---------------|--------------------------------------------------|-------------------|
| **Exploit**   | Uses a vulnerability to get in                   | 🎯 **GET IN**     |
| **Payload**   | Code that runs after successful exploitation     | 💻 **RUN**        |
| **Auxiliary** | Scanning, enumeration, fuzzing, etc.             | 🔧 **HELP**       |
| **Encoder**   | Changes/encodes the payload                      | 🎭 **CHANGE**     |
| **NOP**       | Does nothing; used in NOP sleds                  | 🛝 **DO NOTHING** |
| **Single**    | Complete payload in one piece                    | 📦 **ALL-IN-ONE** |
| **Stager**    | Small payload that sets up the connection        | 🚪 **START**      |
| **Stage**     | Larger payload/functionality delivered afterward | 📦 **MAIN PART**  |

## 14. Exam Traps (read twice) ⚠️

- **Pass-the-Hash** needs the **hash**, not the plaintext password.

- **Golden Ticket** = forged **TGT** (krbtgt). **Silver Ticket** = forged **service ticket**.

- **Responder** = LLMNR/NBT-NS poisoning. **Mimikatz** = pull creds/tickets from memory.

- **Online** cracking (Hydra/Medusa) hits the live service; **offline** (Hashcat/John) cracks stolen hashes.

- **Salting** defeats rainbow tables.

- **SAM** = local hashes; **ntds.dit** = domain (AD) hashes.

- **Vertical** privesc = go up (user→admin); **horizontal** = sideways to another same-level user.

- NTFS **ADS** hides data inside a file; **steganography** hides data inside an image/audio/video.

## 15. Mnemonics

**Sub-phases — “GEMC”:** Gaining, Escalating, Maintaining, Covering.

**Password categories — “NAPO”:** Non-electronic, Active online, Passive online, Offline.

**Tickets:** “Golden = Grand (whole domain, TGT). Silver = Single service.”

## 16. What to Memorize Cold 🎯

- The 4 sub-phases in order: Gaining → Escalating → Maintaining → Covering (GEMC).

- 4 password-attack categories: Non-electronic, Active online, Passive online, Offline (NAPO).

- Cracking techniques: dictionary, brute force, hybrid, rule-based, rainbow table, mask.

- Credential attacks: Pass-the-Hash, Pass-the-Ticket, Kerberoasting, AS-REP roasting, LLMNR poisoning (Responder), Golden/Silver ticket.

- Tools: Hashcat/John (offline), Hydra/Medusa (online), Mimikatz (creds from memory), Responder (LLMNR).

- SAM (local) vs ntds.dit (domain); LM (weak) vs NTLMv2 vs Kerberos.

- Vertical vs Horizontal privilege escalation.

- Rootkit levels; NTFS ADS and steganography for hiding.

- Covering tracks: clear logs (clearev/wevtutil), timestomping, disable auditing (auditpol).

**Exam behavior tip:** System Hacking questions are usually scenario + tool: “the attacker captured a hash and used it to log in without cracking it—what attack?” (Pass-the-Hash). Read for the **keyword** (hash, ticket, LLMNR, service account, timestamp) and match it to the exact attack.
