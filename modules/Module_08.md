**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 08 — Sniffing**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Google Drive (CEH V13 Master Study Guide) + Project documents (08 - Network sniffing.md). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

Sniffing (packet sniffing) means capturing and reading data packets as they travel across a network, using software or a hardware device.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The house/burglar picture</strong></p>
<p><strong>•</strong> Think of a mailroom where every letter passes through. A sniffer is someone secretly reading each letter before it reaches the right person.</p>
<p><strong>•</strong> If the letters are in plain text (no encryption), the reader gets everything: usernames, passwords, messages.</p></td>
</tr>
</tbody>
</table>

# **2) Simple explanation**

- **Promiscuous mode:** the attacker sets the network card (NIC) to listen to ALL traffic on the segment, not just traffic meant for it.

- **Plain text = danger:** protocols that send data unencrypted (Telnet, HTTP, FTP…) are easy to sniff.

- **Where it happens:** the attacker must be on the same network segment (LAN, same Wi-Fi, a mirrored port).

Sniffing gets information, credentials, and passwords — *as long as they're sent **unencrypted** and while they're **moving** through the network.*

*Sniffing is a **passive attack** used in the **Gaining Access** phase to capture credentials and data in transit*

| **Protocol**                     | **Sniffable?**      |
|------------------------------------------------------------|-----------------------------------------------|
| Telnet (23), HTTP (80), FTP (21) | ✅ Yes — plain text |
| SSH (22), HTTPS (443), SFTP      | ❌ No — encrypted   |

|                                              | **Passive sniffing**                 | **Active sniffing**                                                            |
|----------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Where it works     | *HUB networks*                       | *SWITCH networks*                                                              |
| What attacker does | Just listens                         | **Injects packets** into the network                                           |
| Sends traffic?     | ❌ No                                | ✅ Yes                                                                         |
| Detectable?        | Very hard                            | Easier (noisy)                                                                 |
| Examples           | Quiet capture on a hub / mirror port | MAC flooding, ARP poisoning, DHCP attacks, DNS poisoning, switch port stealing |

# **3) Purpose (why attackers sniff)**

- Steal credentials, session cookies, emails, and files in transit.

- Map the network and see who talks to whom.

- Set up bigger attacks: MITM (man-in-the-middle), session hijacking, DNS spoofing.

- **MITM = intercept** \| **Hijacking = steal session** \| **DNS spoofing = redirect**

# **4) How it works — Hub vs Switch**

This is the \#1 thing to understand for the exam. How you sniff depends on the network device.

| **Environment**            | **How data moves**                                                   | **Can you just sniff?**                     |
|----------------------------|----------------------------------------------------------------------|---------------------------------------------|
| Shared Ethernet (HUB)      | Hub sends every packet to EVERY port                                 | YES — passive sniffing works directly       |
| Switched Ethernet (SWITCH) | Switch uses a CAM/MAC table to send packets ONLY to the correct port | NO — you must use active tricks (see below) |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Key exam idea</strong></p>
<p><strong>•</strong> HUB = everyone hears everything → passive sniffing.</p>
<p><strong>•</strong> SWITCH = private delivery → attacker must ACTIVELY inject traffic (MAC flooding, ARP poisoning, etc.) to force the switch to leak.</p>
<p><mark><strong>MAC flooding</strong> and <strong>ARP poisoning</strong> are examples of <strong>active sniffing techniques</strong>.</mark></p>
<ul>
<li><p><mark><strong>MAC flooding</strong> → overloads the switch’s MAC table → traffic may be broadcast more widely → attacker can sniff.</mark></p></li>
<li><p><mark><strong>ARP poisoning</strong> → tricks devices about MAC/IP mappings → attacker gets in the communication path → <strong>MITM + sniffing</strong>.</mark></p></li>
</ul></td>
</tr>
</tbody>
</table>

# **5) Types of sniffing**

| **Type**         | **What it does**                                  | **Sends packets?** | **Examples**                                                                             |
|------------------|---------------------------------------------------|--------------------|------------------------------------------------------------------------------------------|
| Passive sniffing | Just listens, captures traffic. Used on hubs.     | NO                 | Quiet capture on a hub / mirrored port                                                   |
| Active sniffing  | Injects traffic to force a switch to reveal data. | YES                | MAC flooding, ARP poisoning, DHCP attacks, DNS poisoning, switch port stealing, spoofing |

 **MAC flooding** → Overload switch MAC table.

 **ARP poisoning** → Fake ARP mappings → MITM.

 **DHCP attack** → Fake/manipulate DHCP server.

 **DNS poisoning** → Fake DNS answers → redirect.

 **Switch port stealing** → Steal another device’s traffic.

 **Spoofing** → Pretend to be another device/identity.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam trap</strong></p>
<p><strong>•</strong> Passive = sends NO packets (hard to detect). Active = sends packets (noisier, detectable).</p>
<p><strong>•</strong> If the question says 'switched network', the answer is almost always an ACTIVE technique.</p></td>
</tr>
</tbody>
</table>

# **6) The active sniffing attacks (high yield)**

## **MAC Flooding**

- Attacker floods the switch with thousands of fake MAC addresses until the CAM table (***CAM = Content Addressable Memory** → a switch uses it to remember **which MAC address is on which port***) is full.

- The switch fails 'open' and starts acting like a HUB — broadcasting all packets everywhere.

- **Tool:** macof (macof -i eth0). Defense: Port Security.

## **ARP Poisoning / Spoofing**

- ARP maps IP → MAC. ARP (*Address Resolution Protocol* ) is stateless: a machine accepts an ARP reply even if it never asked.

- Attacker sends forged ARP replies linking HIS MAC to the victim's/gateway's IP → traffic flows through him = MITM.

- **Tools:** arpspoof, Ettercap, Cain & Abel. Defense: Dynamic ARP Inspection (DAI)**Dynamic ARP Inspection (DAI)** → **protects against ARP spoofing/poisoning** by checking whether ARP messages are legitimate.

## **DHCP Attacks**

| **Attack**        | **What happens**                                                                           | **Tools**                   | **Defense**                   |
|-------------------|--------------------------------------------------------------------------------------------|-----------------------------|-------------------------------|
| DHCP Starvation   | Floods DHCP server with requests until its IP pool is empty → new clients get no IP        | Yersinia, Hyenae, dhcpstarv | Port Security, DHCP filtering |
| Rogue DHCP Server | Attacker's fake DHCP server answers first, hands victim a malicious default gateway → MITM | mitm6, Ettercap, Gobbler    | DHCP Snooping (trusted ports) |

## **DNS Poisoning**

- Corrupts DNS answers so a correct domain name resolves to the attacker's IP (victim goes to a fake site).

- **Types:** Intranet spoofing (via ARP poisoning), Internet spoofing, Proxy-server poisoning, DNS cache poisoning (e.g. SAD DNS).

- **Defense:** DNSSEC, SSL/TLS.

## **MAC Spoofing / Switch Port Stealing / VLAN Hopping**

- MAC spoofing: attacker copies a victim's MAC to bypass port controls. Defense: DHCP snooping binding table, DAI, IP Source Guard.

- Switch port stealing: forged ARP packets using the victim's MAC to 'steal' its switch port.

- VLAN hopping: Switch spoofing (fake trunk) or Double tagging (802.1Q outer+inner tag) to reach other VLANs.

- STP attack: rogue switch with lowest priority becomes root bridge → all traffic flows through it. Defense: BPDU Guard, Root Guard.

| **Attack**               | **What happens — simple**                                                                              | **Main defense**                    | **What the defense does**                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| **MAC Flooding**         | Attacker sends **thousands of fake MAC addresses** → CAM table becomes full → switch may flood traffic | **Port Security**                   | Limits the **number of MAC addresses** allowed on a switch port. Too many → port can be blocked.                            |
| **ARP Poisoning**        | Attacker sends **fake ARP messages** → victim associates attacker's MAC with gateway IP → **MITM**     | **DAI (Dynamic ARP Inspection)**    | Switch **checks ARP messages** against trusted IP-MAC information and **blocks fake ARP packets**.                          |
| **DHCP Starvation**      | Attacker sends many DHCP requests → DHCP **IP pool becomes empty** → legitimate clients get no IP      | **DHCP filtering / Port Security**  | Limits or filters excessive DHCP requests so an attacker cannot easily **consume all IP addresses**.                        |
| **Rogue DHCP**           | Attacker creates a **fake DHCP server** → gives victims a malicious gateway/DNS                        | **DHCP Snooping**                   | Switch identifies **trusted DHCP ports** and blocks DHCP server replies coming from **untrusted ports**.                    |
| **DNS Poisoning**        | Fake DNS answer → bank.com resolves to **attacker's IP**                                               | **DNSSEC**                          | Uses **digital signatures** to verify that DNS responses are authentic and have not been modified.                          |
| **MAC Spoofing**         | Attacker **copies another device's MAC address**                                                       | **Port Security / IP Source Guard** | Restricts which MAC/IP addresses are allowed on a port and can **block unauthorized traffic**.                              |
| **Switch Port Stealing** | Attacker uses a victim's MAC to **interfere with its switch port**                                     | **Port Security**                   | Restricts the MAC address(es) that can appear on the port.                                                                  |
| **VLAN Hopping**         | Attacker tries to **jump from one VLAN to another**                                                    | **Secure trunk configuration**      | Prevents unauthorized trunking and restricts which VLANs are allowed on trunks.                                             |
| **STP Attack**           | Attacker introduces a rogue switch and tries to become **Root Bridge**                                 | **BPDU Guard / Root Guard**         | **BPDU Guard** blocks a port receiving unexpected BPDUs; **Root Guard** prevents an unauthorized switch from becoming root. |

# **7) Protocols vulnerable to sniffing (plain text)**

These send data with NO encryption, so anything captured is readable. Memorize this list.

| **Protocol** | **Port**  | **Why vulnerable**                |
|--------------|-----------|-----------------------------------|
| Telnet       | 23        | Login + commands in clear text    |
| HTTP         | 80        | Web data unencrypted              |
| FTP          | 20/21     | Credentials + files in clear text |
| SMTP         | 25        | Email sent in clear text          |
| POP3 / IMAP  | 110 / 143 | Email retrieval unencrypted       |
| SNMP         | 161/162   | Community strings exposed         |
| TFTP         | 69        | No authentication, clear text     |
| NNTP         | 119       | News transfer in clear text       |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook — the plain-text family</strong></p>
<p><strong>•</strong> 'Fat Tigers Hate Sniffing Plain Text Snacks' → FTP, Telnet, HTTP, SMTP, POP/IMAP, TFTP, SNMP.</p>
<p><strong>•</strong> Secure twins to remember: SSH (22) not Telnet, HTTPS (443) not HTTP, SFTP/FTPS not FTP.</p></td>
</tr>
</tbody>
</table>

**Plain text = no encryption → sniffed traffic may be readable.**

| **Insecure Protocol** | **Port** | **Why vulnerable?**                                  | **Secure version**   |
|-----------------------|----------|------------------------------------------------------|----------------------|
| Telnet                | 23       | Login + commands sent in clear text                  | SSH (22)             |
| HTTP                  | 80       | Web data sent without encryption                     | HTTPS (443)          |
| FTP                   | 20/21    | Credentials + files sent in clear text               | SFTP / FTPS          |
| SMTP                  | 25       | Email can be sent without encryption                 | SMTP + TLS           |
| POP3                  | 110      | Email retrieval + credentials unencrypted            | POP3S (995)          |
| IMAP                  | 143      | Email retrieval + credentials unencrypted            | IMAPS (993)          |
| SNMP v1/v2c           | 161/162  | Community strings and management data can be exposed | SNMPv3               |
| TFTP                  | 69       | No authentication + clear text                       | Secure file transfer |
| NNTP                  | 119      | News/article transfer in clear text                  | NNTPS (563)          |

**TLS (Transport Layer Security) is a security protocol that encrypts data while it travels between two devices.**

# **8) Tools (know tool → purpose)**

| **Tool**         | **Purpose**                                             | **Exam takeaway**                       |
|------------------|---------------------------------------------------------|-----------------------------------------|
| Wireshark        | GUI packet capture + deep analysis (uses WinPcap/Npcap) | The \#1 sniffer. Knows display filters. |
| tcpdump          | Command-line packet capture (Linux)                     | CLI cousin of Wireshark                 |
| Ettercap         | MITM + ARP poisoning framework                          | Active sniffing / MITM                  |
| Cain & Abel      | Windows sniffing + ARP poisoning + password recovery    | Classic Windows tool                    |
| macof            | MAC flooding (fills CAM table)                          | MAC flooding keyword                    |
| SSLstrip         | Downgrades HTTPS to HTTP to read traffic                | HTTPS downgrade attack                  |
| dsniff           | Suite of sniffing/credential tools                      | Password sniffing                       |
| Yersinia         | Layer-2 attacks (DHCP, STP, VLAN)                       | DHCP starvation keyword                 |
| Capsa / OmniPeek | Network analyzers (also help detect sniffing)           | Analysis + detection                    |

| **Tool** | **What it does**                          | **When / Why use it?**                                                                        | **Exam takeaway**                        |
|------------------------------------|---------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| **Wireshark**                      | GUI packet capture + deep packet analysis                           | **When you want to see and analyze network traffic visually** — protocols, IPs, TCP/UDP, credentials in cleartext, etc. | ⭐ **\#1 packet analyzer / sniffer**; remember **display filters** |
| **tcpdump**                        | Command-line packet capture and analysis                            | **When you need packet capture from CLI**, especially on Linux/servers without a GUI                                    | **CLI cousin of Wireshark**                                        |
| **Ettercap**                       | MITM framework; ARP poisoning; traffic interception                 | **When performing an active MITM attack**, especially ARP poisoning                                                     | **MITM / ARP poisoning**                                           |
| **Cain & Abel**                    | Windows password recovery + sniffing + ARP poisoning                | **When attacking/analyzing Windows networks**, especially older/classic CEH scenarios                                   | **Classic Windows sniffing/password tool**                         |
| **macof**                          | Generates many fake MAC addresses to flood a switch's CAM table     | **When demonstrating MAC flooding** and potentially causing traffic to be broadcast/flooded                             | ⭐ **MAC flooding = macof**                                        |
| **SSLstrip**                       | Attempts to downgrade HTTPS connections to HTTP                     | **When demonstrating an HTTPS downgrade attack** against vulnerable/non-HSTS situations                                 | ⭐ **HTTPS → HTTP downgrade**                                      |
| **dsniff**                         | Collection of network sniffing/credential-related tools             | **When looking for credentials/protocol information sent over insecure protocols**                                      | **Password sniffing suite**                                        |
| **Yersinia**                       | Attacks Layer-2 protocols such as DHCP, STP, VLAN-related protocols | **When testing Layer-2 infrastructure/protocol weaknesses**                                                             | ⭐ **DHCP starvation / Layer-2 attacks**                           |
| **Capsa**                          | GUI network analyzer/sniffer                                        | **When you want GUI-based network monitoring and traffic analysis**                                                     | **Network analyzer**                                               |
| **OmniPeek**                       | Network packet analyzer/capture tool                                | **When analyzing network traffic and troubleshooting/monitoring**                                                       | **Network analysis / sniffing**                                    |

# **9) Commands & Wireshark filters (authorized labs only)**

Wireshark uses DISPLAY FILTERS (what you see) — memorize a few common ones.

| **Goal**                  | **Filter / Command**                    |
|---------------------------|-----------------------------------------|
| Only HTTP GET requests    | http.request.method == "GET"            |
| Traffic to/from one host  | ip.addr == 10.0.0.4                     |
| Only Telnet (port 23)     | tcp.port == 23                          |
| SMTP (25) OR ICMP         | tcp.port eq 25 or icmp                  |
| All TCP resets            | tcp.flags.reset == 1                    |
| Hide arp/icmp/dns noise   | !(arp or icmp or dns)                   |
| MAC flooding (Linux, lab) | macof -i eth0                           |
| Detect sniffers with Nmap | nmap --script=sniffer-detect \<target\> |

# **10) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> A tester is on a SWITCHED LAN and wants to read a colleague's HTTP traffic.</p>
<p><strong>•</strong> Step 1 — Switch blocks plain sniffing, so he uses ARP poisoning (Ettercap) to become MITM between the victim and the gateway.</p>
<p><strong>•</strong> Step 2 — All victim traffic now flows through his machine.</p>
<p><strong>•</strong> Step 3 — He opens Wireshark, filters http.request, and reads the plain-text logins.</p>
<p><strong>•</strong> Fix: the site should use HTTPS, and the switch should enable Dynamic ARP Inspection.</p></td>
</tr>
</tbody>
</table>

# **11) Detection & countermeasures**

## **How to DETECT a sniffer**

- **Ping method:** ping a suspect using the right IP but a WRONG MAC — only a NIC in promiscuous mode replies.

- **ARP method:** send a non-broadcast ARP; a sniffing host in promiscuous mode still responds.

- **DNS method:** watch for machines doing lots of reverse-DNS lookups on captured IPs.

- **Tools:** nmap --script=sniffer-detect, NetScanTools Pro; also IDS.

**🕵️ How to detect a sniffer**

| **Method** | **What you do**           | **Why it can detect a sniffer**                        | **Active / Passive?** |
|--------------------------------------|-----------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------------------------|
| **Ping method**                      | Send packet to the correct IP but **wrong MAC**     | A NIC in **promiscuous mode** may process/respond to traffic not addressed to it | 🔴 **Active detection**                         |
| **ARP method**                       | Send a **non-broadcast ARP** request                | A promiscuous-mode host may respond even though the ARP wasn't meant for it      | 🔴 **Active detection**                         |
| **DNS method**                       | Monitor for excessive reverse-DNS lookups           | May indicate a machine investigating/correlating captured IP addresses           | 🟢 **Passive detection**                        |
| **Tools**                            | nmap --script=sniffer-detect, NetScanTools Pro, IDS | Automate/monitor detection of suspicious sniffing behavior                       | Depends on technique                            |

## **How to PREVENT sniffing**

- Use end-to-end encryption: HTTPS, SSH, SFTP, VPN, TLS everywhere.

- Restrict physical access to the network and switch ports.

- Enable Port Security, Dynamic ARP Inspection, DHCP Snooping, IP Source Guard on switches.

- Use static ARP entries for critical hosts.

# **12) Related concepts**

| **Concept**           | **One-line meaning**                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------|
| Promiscuous mode      | NIC listens to ALL traffic on its segment (needed to sniff).                                |
| Monitor mode          | Wireless equivalent — captures raw radio frames (Wi-Fi). Do NOT confuse with promiscuous.   |
| SPAN / Port Mirroring | Cisco switch feature copying traffic to one port. An attacker on that port sees everything. |
| Wiretapping           | Tapping phone/comm lines. Active = alter data (MITM); Passive = just eavesdrop.             |
| CAM table             | The switch's MAC-to-port table. MAC flooding overflows it.                                  |

# **13) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Passive vs Active: passive sends NO packets; active injects traffic. 'Switched network' → active.</p>
<p><strong>•</strong> Promiscuous (wired) vs Monitor mode (wireless) — different things.</p>
<p><strong>•</strong> MAC flooding makes a SWITCH behave like a HUB (not the other way round).</p>
<p><strong>•</strong> ARP is STATELESS — that's WHY spoofing works (replies accepted without a request).</p>
<p><strong>•</strong> DHCP Starvation = empties the IP pool; Rogue DHCP = hands out a malicious gateway. Don't swap them.</p>
<p><strong>•</strong> Wireshark uses DISPLAY filters to view; capture filters are set before capture — questions may test the difference.</p></td>
</tr>
</tbody>
</table>

# **14) Mnemonics**

- **HUB vs SWITCH:** 'Hub = Hears all, Switch = Sends selective.'

- **Active attacks:** 'My Angry Dog Digs Sniffing Spots' → MAC flooding, ARP poisoning, DHCP attacks, DNS poisoning, Switch port stealing, Spoofing.

- **Plain-text protocols:** 'Fat Tigers Hate Sniffing Plain Text Snacks' → FTP, Telnet, HTTP, SMTP, POP/IMAP, TFTP, SNMP.

- **ARP works because:** 'No question needed' — ARP is stateless.

# **15) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Sniffing = capturing packets; needs promiscuous mode + same segment.</p>
<p><strong>•</strong> HUB → passive works. SWITCH → need active (MAC flood / ARP poison).</p>
<p><strong>•</strong> Passive = no packets sent. Active = packets sent.</p>
<p><strong>•</strong> MAC flooding: macof; overflows CAM table; switch → hub. Defense: Port Security.</p>
<p><strong>•</strong> ARP poisoning: forged replies → MITM. Defense: Dynamic ARP Inspection.</p>
<p><strong>•</strong> DHCP Starvation (empties pool) vs Rogue DHCP (fake gateway). Defense: DHCP Snooping.</p>
<p><strong>•</strong> DNS poisoning defense: DNSSEC.</p>
<p><strong>•</strong> Plain-text ports: Telnet 23, HTTP 80, FTP 20/21, SMTP 25, POP3 110, IMAP 143, SNMP 161, TFTP 69.</p>
<p><strong>•</strong> Top tools: Wireshark, tcpdump, Ettercap, Cain &amp; Abel, macof, SSLstrip.</p>
<p><strong>•</strong> Detect: ping/ARP/DNS methods + nmap --script=sniffer-detect.</p>
<p><strong>•</strong> Prevent: encryption everywhere (HTTPS/SSH/VPN) + switch hardening.</p></td>
</tr>
</tbody>
</table>

*End of Module 08 — Sniffing. Next up: quiz this module, or move to Module 09 (Social Engineering).*

If PC C's NIC is in **promiscuous mode**, it can capture traffic that reaches its network interface even when the destination MAC is **PC B**, not PC C.
