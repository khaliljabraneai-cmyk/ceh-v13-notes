**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 09 — Social Engineering**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Google Drive (CEH V13 Master Study Guide, OhPhish manual) + Project documents (09 - Social Engineering.md). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

Social engineering is the art of TRICKING PEOPLE into giving away confidential information or doing something unsafe — instead of attacking the computer, the attacker attacks the human.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The con-artist picture</strong></p>
<p><strong>•</strong> A burglar could pick your lock (technical hacking)… or he could dress as a delivery man and politely ask you to open the door (social engineering).</p>
<p><strong>•</strong> The door is the same. The trick is aimed at YOU, not the lock.</p>
<p><strong>•</strong> That's why it's called 'hacking the human'.</p></td>
</tr>
</tbody>
</table>

# **2) Simple explanation**

- **Targets the human, not the machine:** no exploit, no malware needed (though it can carry them).

- **Exploits trust and emotion:** fear, urgency, curiosity, greed, and the wish to be helpful.

- **Why it works:** humans are the weakest link. There is no patch for human behavior — only awareness training.

# **3) Purpose (why attackers use it)**

- Steal credentials, OTPs, and sensitive data directly from a person.

- Get a first foot in the door (initial access) before technical attacks.

- Bypass strong technical controls (firewalls, encryption) by fooling an insider.

# **4) How it works — the attack cycle**

Most social engineering follows four steps. Memorize the order.

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 22%" />
<col style="width: 68%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Step</strong></th>
<th><strong>Phase</strong></th>
<th><strong>What the attacker does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1</td>
<td>Research</td>
<td><p>Gather info on the target (footprinting, dumpster diving, OSINT)</p>
<p><strong>Footprinting = Map the target (Find IP ranges, domains, employees, technologies)</strong><br />
<strong>OSINT = Search public information(Google, LinkedIn, company website, public databases)</strong><br />
<strong>Dumpster Diving = Search the trash(Old documents, printed reports, discarded storage devices°</strong></p></td>
</tr>
<tr class="even">
<td>2</td>
<td>Select target</td>
<td>Pick a victim — often a helpful or frustrated employee</td>
</tr>
<tr class="odd">
<td>3</td>
<td>Build relationship</td>
<td>Gain trust — impersonate, befriend, create urgency</td>
</tr>
<tr class="even">
<td>4</td>
<td>Exploit</td>
<td>Extract info or make the victim act (click, pay, reveal)</td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam anchor</strong></p>
<p><strong>•</strong> Research → Select → Relationship → Exploit.</p>
<p><strong>•</strong> Social engineering is a CYCLE — the attacker may loop back and reuse info on new victims.</p></td>
</tr>
</tbody>
</table>

# **5) Types of social engineering**

Three big families. This grouping is the \#1 exam structure.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 33%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Family</strong></th>
<th><strong>Meaning</strong></th>
<th><strong>Examples</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Human-based</td>
<td><p>Person-to-person deception</p>
<p>Deceiving or interacting with <strong>people</strong></p></td>
<td>Impersonation, tailgating, shoulder surfing, dumpster diving, eavesdropping</td>
</tr>
<tr class="even">
<td>Computer-based</td>
<td><p>Using software / the internet</p>
<p>Using <strong>computers/software/ Internet</strong> to deceive users</p></td>
<td>Phishing, spam, pop-ups, scareware, pharming</td>
</tr>
<tr class="odd">
<td>Mobile-based</td>
<td><p>Using apps / phones</p>
<p>Using <strong>mobile phones/apps</strong> to deceive users</p></td>
<td>Malicious apps, smishing (SMS), fake security apps</td>
</tr>
</tbody>
</table>

## **Human-based techniques (know these cold)**

| **Technique**              | French                                           | **Simple meaning**                                                                        |
|----------------------------|----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| Impersonation              | Usurpation d’identité                            | Pretend to be someone trusted (IT, boss, vendor)                                          |
| Tailgating                 | Suivi / entrée en suivant une personne autorisée | Follow an authorized person through a secure door WITHOUT a badge (attacker has no badge) |
| Piggybacking               | Entrée avec l’autorisation d’une personne        | Authorized person KNOWINGLY lets you in (with consent)                                    |
| Shoulder surfing           | Observation par-dessus l’épaule                  | Watch over someone's shoulder to steal passwords/PINs                                     |
| Dumpster diving            | Fouille des poubelles                            | Search the trash for useful info (papers, disks)                                          |
| Eavesdropping              | Écoute clandestine                               | Secretly listen to conversations                                                          |
| Reverse social engineering | Ingénierie sociale inversée                      | Attacker makes the victim ASK him for help, then extracts info                            |
| Diversion theft            | Détournement de livraison                        | Trick a delivery/courier into delivering to the wrong place                               |
| Quid pro quo               | Donnant-donnant / échange de services            | Offer a 'service' (fake IT help) in exchange for credentials                              |
| Elicitation                | Extraction d’informations                        | Casual, friendly conversation designed to pull out information                            |
| Honey trap                 | Piège sentimental                                | Fake romantic/online relationship to extract secrets                                      |
| Baiting                    | Appât / piège par appât                          | Leave infected USB/'Free' item for a curious victim to pick up                            |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam trap — Tailgating vs Piggybacking</strong></p>
<p><strong>•</strong> Tailgating = WITHOUT consent (sneak in behind someone).</p>
<p><strong>•</strong> Piggybacking = WITH consent (the person holds the door for you).</p>
<p><strong>•</strong> Both = physically entering behind an authorized person.</p></td>
</tr>
</tbody>
</table>

# **6) Phishing family (very high yield)**

Phishing = fake message that looks legitimate, to steal data or deliver malware. Know every variant by its target/channel.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 45%" />
<col style="width: 34%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Type</strong></th>
<th><strong>How it's different</strong></th>
<th><strong>Channel / Target</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Phishing</td>
<td>Mass fake emails to anyone</td>
<td>Email → fake website</td>
</tr>
<tr class="even">
<td>Spear phishing</td>
<td>Targeted at a SPECIFIC person/org</td>
<td>Personalized email</td>
</tr>
<tr class="odd">
<td>Whaling</td>
<td>Targets a BIG FISH (CEO, CFO, executive)</td>
<td>Executive email</td>
</tr>
<tr class="even">
<td>Vishing</td>
<td>Voice phishing over a phone call</td>
<td>Phone / VoIP</td>
</tr>
<tr class="odd">
<td>Smishing</td>
<td>Phishing by SMS text message</td>
<td>SMS</td>
</tr>
<tr class="even">
<td>Pharming</td>
<td><p>Redirects to fake site via DNS/host-file poisoning</p>
<p><mark>Redirects you to a fake site <strong>without needing a phishing link</strong></mark></p></td>
<td>DNS — no click needed</td>
</tr>
<tr class="odd">
<td>Spimming</td>
<td>Spam over instant messaging</td>
<td>IM apps</td>
</tr>
<tr class="even">
<td>Clone phishing</td>
<td><p>Copy of a real email with a swapped link</p>
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
<th><mark>Copies a legitimate email but replaces the link/attachment</mark></th>
</tr>
</thead>
<tbody>
</tbody>
</table></td>
<td>Email</td>
</tr>
<tr class="odd">
<td>Angler phishing</td>
<td>Fake support account on social media</td>
<td>Social media</td>
</tr>
<tr class="even">
<td>Consent phishing</td>
<td><p>Abuses OAuth 'allow access' prompts</p>
<p><mark>Tricks user into granting a malicious app <strong>OAuth permissions</strong></mark></p></td>
<td>App permissions</td>
</tr>
<tr class="odd">
<td>QRLjackING</td>
<td><p>Malicious QR code hijacks a session</p>
<p><mark>Uses a malicious QR code to redirect/hijack access</mark></p></td>
<td>QR code</td>
</tr>
<tr class="even">
<td>Tabnabbing</td>
<td><p>Idle browser tab silently becomes a fake login</p>
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
<th><mark>Changes an inactive browser tab into a fake login page</mark></th>
</tr>
</thead>
<tbody>
</tbody>
</table></td>
<td>Browser tab</td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Don't confuse these</strong></p>
<p><strong>•</strong> Spear phishing = specific person. Whaling = specific EXECUTIVE. Whaling is a type of spear phishing.</p>
<p><strong>•</strong> Vishing = Voice (call). Smishing = SMS (text).</p>
<p><strong>•</strong> Pharming needs NO click — it poisons DNS so the correct URL leads to a fake site.</p>
<p><strong>•</strong> Phishing is SOCIAL ENGINEERING — it needs NO malware and NO server exploit to work.</p></td>
</tr>
</tbody>
</table>

# 

# **7) Insider threats & identity theft**

## **Insider threat**

- A threat coming from INSIDE the organization (employee, contractor, partner).

- Types: malicious insider (angry/greedy), negligent insider (careless), compromised insider (account stolen), professional/third-party.

- **Why dangerous:** insiders already have trust and access — they bypass perimeter defenses.

## **Identity theft**

- Stealing someone's personal info (name, ID, SSN) to impersonate them for fraud.

- **Feeds attacks:** stolen identity is used to reset passwords, open accounts, or pass impersonation checks.

# **8) Ports / protocols**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Reality check</strong></p>
<p><strong>•</strong> Social engineering targets PEOPLE, so it has no ports of its own.</p>
<p><strong>•</strong> But its payloads ride normal channels: Email SMTP 25 / IMAP 143 / POP3 110, Web HTTP 80 / HTTPS 443, DNS 53 (pharming).</p>
<p><strong>•</strong> Exam tip: if a question asks 'which port does phishing use', think of the delivery channel (usually email/web), not a magic 'phishing port'.</p></td>
</tr>
</tbody>
</table>

# **9) Tools (know tool → purpose)**

| **Tool**        | **Category** | **Free?** | **When is it used?** | **Why is it used?** | **🧠 Simple real example** |
|-------------------------------------------|----------------------------------------|-------------------------------------|------------------------------------------------|-----------------------------------------------|------------------------------------------------------|
| **SET**                                   | 🎣 Phishing / Social Engineering       | Free / Open-source                  | Security testing                               | Create/test social-engineering attacks        | Tester creates a fake Microsoft login page           |
| **ShellPhish**                            | 🎣 Fake Login Page                     | Free                                | Phishing-page testing                          | Quickly create fake login pages               | Creates a fake Instagram login page                  |
| **OhPhish**                               | 🎓 Phishing Training                   | Depends                             | Awareness training                             | Test if employees recognize phishing          | Company sends a fake phishing email                  |
| **Netcraft**                              | 🛡️ Phishing Detection                  | Free options                        | Visiting websites                              | Detect suspicious websites                    | Warns you that a website may be dangerous            |
| **PhishTank**                             | 🛡️ Phishing Database                   | Free                                | Checking URLs                                  | Check if a URL is reported as phishing        | Check fake-bank.com                                  |
| **Synthesia**                             | 🎥 Fake Video / AI Avatar              | Limited free + paid                 | AI video creation                              | Create realistic AI videos                    | Fake CEO video gives instructions                    |
| **DeepFaceLab**                           | 🎥 Fake Video / Face Swap              | Free / Open-source                  | Deepfake creation/testing                      | Swap faces in videos                          | Put CEO's face on another person's video             |
| **ElevenLabs**                            | 🔊 Fake Voice                          | Free plan + paid                    | Voice generation/cloning                       | Create realistic AI voices                    | Fake voice sounds like the manager                   |
| **Resemble AI** | 🔊 Fake Voice                          | Free/pay-as-you-go + paid           | Voice AI/testing                               | Generate or clone voices                      | Test AI-generated voice impersonation                |
| **QR TIGER**    | 📱 QR Code                             | Free options + paid                 | Creating QR codes                              | Generate QR codes                             | QR code sends victim to a phishing site              |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>V13 AI note</strong></p>
<p><strong>•</strong> CEH v13 adds AI-powered social engineering: deepfake video and cloned voices make vishing/whaling far more convincing. Expect a question on AI + social engineering.</p></td>
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
<p><strong>•</strong> An attacker calls the help desk pretending to be a senior manager locked out before a big meeting (impersonation + urgency).</p>
<p><strong>•</strong> He already knows the manager's name and project from LinkedIn (research phase).</p>
<p><strong>•</strong> The stressed help-desk agent resets the password to 'help' quickly (exploit phase).</p>
<p><strong>•</strong> Attacker logs in — no firewall or malware was ever touched.</p>
<p><strong>•</strong> Fix: verify identity via call-back to a known number + strict reset policy + awareness training.</p></td>
</tr>
</tbody>
</table>

# **11) Detection & countermeasures**

- **People:** security-awareness training, simulated phishing (OhPhish), a 'verify before trust' culture.

- **Policy:** identity verification / call-back, clear reset rules, need-to-know access, clean-desk & shredding policy.

- **Technical:** email filters + SPF/DKIM/DMARC, MFA (stops stolen-password reuse), anti-phishing toolbars, URL checks.

- **Physical:** badges, mantraps (stop tailgating), visitor escorts, CCTV.

# **12) Related concepts**

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 32%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th><strong><mark>Concept</mark></strong></th>
<th><strong><mark>One-line meaning</mark></strong></th>
<th><strong><mark>Simple context / example</mark></strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Pretexting</strong></td>
<td>Inventing a believable fake story/role to justify a request</td>
<td>🎭 Attacker pretends to be <strong>IT support</strong> and asks an employee for their login information “to fix an issue.”</td>
</tr>
<tr class="even">
<td><strong>Scareware</strong></td>
<td>Fake warning that says your device is infected and pushes a bad action</td>
<td>😱 A pop-up says <strong>“⚠️ Your computer has 5 viruses! Download this antivirus now!”</strong> The download is malicious.</td>
</tr>
<tr class="odd">
<td><strong>Watering Hole</strong></td>
<td>Infecting a website that the target group is known to visit</td>
<td>💧 Attackers know employees often visit a specific industry website, so they compromise that website and infect visitors.</td>
</tr>
<tr class="even">
<td><strong>Mantrap</strong></td>
<td>Two-door security booth that allows one person at a time</td>
<td><p>🚪 You enter Door 1 → Door 1 locks → Door 2 opens. A second person cannot follow you through (<strong>prevents tailgating</strong>).</p>
<p><strong>Mantrap prevents tailgating</strong></p></td>
</tr>
<tr class="odd">
<td><strong>OSINT</strong></td>
<td>Gathering information from public/open sources</td>
<td>🔎 Attacker researches a company using its <strong>website, LinkedIn, public documents, social media, DNS records</strong>, etc., before an attack.</td>
</tr>
</tbody>
</table>

# **13) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Tailgating (no consent) vs Piggybacking (with consent).</p>
<p><strong>•</strong> Spear phishing (specific person) vs Whaling (specific executive).</p>
<p><strong>•</strong> Vishing = voice call; Smishing = SMS. Don't swap them.</p>
<p><strong>•</strong> Pharming needs NO click — DNS/host-file poisoning redirects a correct URL.</p>
<p><strong>•</strong> Phishing is social engineering — needs NO malware or server exploit to succeed.</p>
<p><strong>•</strong> Quid pro quo (offer a service for data) vs Baiting (leave a tempting item).</p>
<p><strong>•</strong> Reverse social engineering = the VICTIM contacts the attacker for 'help'.</p>
<p><strong>•</strong> Best defense against social engineering = user AWARENESS TRAINING (not a firewall).</p></td>
</tr>
</tbody>
</table>

# **14) Mnemonics**

- **Attack cycle:** 'Really Sneaky Rats Exploit' → Research, Select, Relationship, Exploit.

- **Three families:** 'Humans Click Mobiles' → Human-based, Computer-based, Mobile-based.

- **Phishing channels:** 'Voice=Vishing, SMS=Smishing, DNS=Pharming.'

- **Tail vs Piggy:** 'Tailgating = Thief (no consent). Piggyback = Permission.'

- **Weakest link:** 'No patch for humans' → training is the fix.

# **15) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Social engineering = hacking the human; exploits trust/emotion; humans = weakest link.</p>
<p><strong>•</strong> Cycle: Research → Select target → Build relationship → Exploit.</p>
<p><strong>•</strong> 3 families: Human-based, Computer-based, Mobile-based.</p>
<p><strong>•</strong> Human: impersonation, tailgating (no consent), piggybacking (consent), shoulder surfing, dumpster diving, quid pro quo, baiting, reverse SE.</p>
<p><strong>•</strong> Phishing types: spear (person), whaling (exec), vishing (call), smishing (SMS), pharming (DNS, no click), clone, angler, consent, QRLjacking.</p>
<p><strong>•</strong> Insider threat = danger from inside (already trusted). Identity theft feeds impersonation.</p>
<p><strong>•</strong> Tools: SET (main attack), ShellPhish, OhPhish (training/defense), Netcraft/PhishTank (detect).</p>
<p><strong>•</strong> V13 adds AI: deepfake video + voice cloning for vishing/whaling.</p>
<p><strong>•</strong> Defense: awareness training + MFA + identity verification + email auth (SPF/DKIM/DMARC) + mantraps.</p>
<p><strong>•</strong> Phishing = social engineering: NO malware or exploit required.</p></td>
</tr>
</tbody>
</table>

*End of Module 09 — Social Engineering. Next up: quiz this module, or move to Module 10 (Denial of Service).*
