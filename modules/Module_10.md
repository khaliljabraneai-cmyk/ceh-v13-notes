**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 10 — Denial of Service (DoS / DDoS)**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Google Drive (CEH V13 Master Study Guide) + Project documents (10 - Denial of service.md). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

**A Denial-of-Service (DoS)** attack floods a system, service, or network with so much traffic or so many requests that it slows down or crashes — real users can no longer use it.

**A Distributed Denial-of-Service (DDoS)** attack does the same thing **from MANY machines at once** (a botnet), making it far bigger and harder to block.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The shop-door picture</strong></p>
<p><strong>•</strong> Imagine 1,000 fake customers crowding a shop doorway so real customers can't get in.</p>
<p><strong>•</strong> DoS = one troublemaker blocking the door. DDoS = a whole crowd (botnet) blocking it.</p>
<p><strong>•</strong> The goal is not to steal — it's to make the service UNAVAILABLE (the 'A' in CIA).</p>
<ul>
<li><p><mark><strong>botnet</strong> is a <strong>group of infected/compromised devices controlled by an attacker</strong></mark></p></li>
</ul></td>
</tr>
</tbody>
</table>

# **2) Simple explanation**

- **Target = Availability:** DoS attacks the 'A' of the CIA triad (Confidentiality, Integrity, AVAILABILITY).

- **DoS vs DDoS:** DoS = 1 source. DDoS = many sources (a botnet of 'zombies').

- **No data stolen (usually):** the aim is downtime, disruption, or ransom — not theft.

# **3) Purpose (why attackers do it)**

- Take a website/service offline (revenge, protest 'hacktivism', competition).

- Extort money — 'pay or we keep you down' (Ransom DDoS / RDDoS).

- Distraction — flood the SOC while a real breach happens elsewhere.

# **4) How DDoS works — the botnet**

A botnet is a network of infected machines ('zombies' / 'bots') controlled by the attacker through a Command & Control (C2) server. They all attack the victim at once.

## **How a botnet finds new machines (scanning methods)**

<table>
<colgroup>
<col style="width: 27%" />
<col style="width: 72%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Method</strong></th>
<th><strong>How it finds victims</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Random scanning</td>
<td>Picks random IP addresses to probe for vulnerabilities</td>
</tr>
<tr class="even">
<td>Hit-list scanning</td>
<td>Pre-built list of vulnerable machines, infected fast to form the zombie army</td>
</tr>
<tr class="odd">
<td>Topological scanning</td>
<td>Uses info from an already-infected machine to find its neighbors</td>
</tr>
<tr class="even">
<td>Local subnet scanning</td>
<td>Infects machines on the same local network</td>
</tr>
<tr class="odd">
<td>Permutation scanning</td>
<td><p>Shared pseudo-random list of all IPs to avoid re-scanning</p>
<p>There are many Bots Bots share the IP search to avoid scanning the same targets.</p></td>
</tr>
</tbody>
</table>

## **How malicious code spreads (propagation)**

- **Propagation helps BUILD the botnet → the botnet can then perform DDoS.**

- **botnet is a group of infected/compromised devices controlled by an attacker**

| **Model** | **Very easy explanation**                                                   | **🧠 Remember** |
|-------------------------------------|-------------------------------------------------------------------------------------------------------|-------------------------------------------|
| **Central source**                  | **The victim downloads the malware from a central server.**                                           | **🌐 From central server**                |
| **Back-chaining**                   | **The victim gets the malware from the attacking/infecting machine.**                                 | **↩️ From attacker**                      |
| **Autonomous**                      | **The attacker puts/injects the malware directly during the attack. No separate download is needed.** | **💉 Injected directly**                  |

# **5) The 3 categories of DoS/DDoS attacks**

This is the \#1 exam structure. Every attack technique fits one of these three.

| **Category**                | **What it exhausts**                               | **Examples**                                           |                                                                                                            |
|-----------------------------|----------------------------------------------------|--------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| Volumetric (bandwidth)      | Fills the pipe with sheer volume of traffic        | UDP flood, ICMP flood, amplification (NTP, DNS), Smurf | Sends **huge amounts of traffic** until the network bandwidth is full.           |
| Protocol (state)            | Eats connection-state resources (tables, memory)   | SYN flood, Ping of Death, fragmentation, Smurf         | Uses/exhausts **network device resources** such as connection tables and memory. |
| Application layer (Layer 7) | Exhausts the web app with 'legit-looking' requests | Slowloris, HTTP GET/POST flood, HTTP/2 Rapid Reset     | Sends many requests to **the application/web server**, consuming its resources.  |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam anchor</strong></p>
<p><strong>•</strong> Volumetric is measured in bits per second (bps). Application-layer is measured in requests per second (rps).</p>
<p><strong>•</strong> Amplification/reflection attacks are volumetric and abuse STATELESS UDP services (NTP, DNS, SSDP).</p></td>
</tr>
</tbody>
</table>

# **6) Key attack techniques (high yield)**

| **Category** | **Attack** | **How it works — easy explanation**                                                           | **Key clue** |
|----------------------------------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------|----------------------------------------|
| 🟢 **Volumetric**                      | **UDP flood**                        | Sends a huge amount of UDP traffic to the target.                                                                       | **UDP + volume**                       |
| 🟢 **Volumetric**                      | **ICMP flood**                       | Sends a huge number of ping requests.                                                                                   | **Ping flood**                         |
| 🟢 **Volumetric**                      | **NTP amplification**                | Small spoofed request causes a much larger NTP response to hit the victim.                                              | **Small → BIG**                        |
| 🟢 **Volumetric**                      | **DNS amplification**                | Small spoofed DNS query causes a large DNS response to hit the victim.                                                  | **Small → BIG**                        |
| 🟢 **Volumetric**                      | **Pulse wave**                       | Sends repeated bursts of traffic instead of one continuous flood.                                                       | **Repeated bursts**                    |
| 🔵 **Protocol**                        | **SYN flood**                        | Sends many SYN requests; the server keeps many connections waiting for completion.                                      | **Half-open connections**              |
| 🔵 **Protocol**                        | **Ping of Death**                    | Sends an oversized/malformed packet that can crash or disrupt the target.                                               | **Oversized packet**                   |
| 🔵 **Protocol**                        | **Smurf attack**                     | Spoofs the victim's IP and sends ICMP to a broadcast address; many hosts reply to the victim.                           | **Broadcast + ICMP + spoofing**        |
| 🔵 **Protocol**                        | **Fragmentation**                    | Sends fragmented packets that the target must reassemble, consuming resources or exploiting packet-handling weaknesses. | **Fragments**                          |
| 🔵 **Protocol / Device-level**         | **Permanent DoS (Phlashing)**        | Malicious firmware/software can permanently disable or brick a device.                                                  | **Permanent damage**                   |
| 🟠 **Application Layer (L7)**          | **Slowloris**                        | Opens many HTTP connections and keeps them alive with very slow requests.                                               | **Slow + HTTP**                        |
| 🟠 **Application Layer (L7)**          | **HTTP GET/POST flood**              | Sends huge numbers of HTTP requests to consume web-server resources.                                                    | **Many HTTP requests**                 |
| 🟠 **Application Layer (L7)**          | **HTTP/2 Rapid Reset**               | Quickly creates and cancels many HTTP/2 streams, consuming server resources.                                            | **HTTP/2 + cancel**                    |
| ⚠️ **Depends on vulnerability**        | **Zero-day DoS/DDoS**                | Uses a previously unknown or unpatched vulnerability to cause DoS/DDoS.                                                 | **No known patch**                     |

# **7) Reflection vs Amplification (don't confuse)**

| **Concept**        | **Meaning**                                                                                         |
|--------------------|-----------------------------------------------------------------------------------------------------|
| Reflection (DRDoS) | Attacker spoofs the victim's IP so innocent 'reflector' servers send their replies TO the victim    |
| Amplification      | The reply is much BIGGER than the request (small query → huge answer), multiplying the traffic      |
| Combined           | Most big DDoS = reflection + amplification using stateless UDP services (NTP, DNS, SSDP, memcached) |

# 

| **Method**                 | **Simple explanation**                                                                         |
|----------------------------|------------------------------------------------------------------------------------------------|
| Botnet                     | Attacker controls many infected devices, which attack the victim together. → **Infected army** |
| Reflection                 | Attacker makes other servers send their responses to the victim. → **Servers send to victim**  |
| Amplification              | A small request causes a much larger response. **→ Small → BIG**                               |
| Reflection + Amplification | Other servers send a much larger response to the victim. **→ Server → BIG traffic → Victim**   |

# **8) Ports / protocols to remember**

| **Protocol / port** | **Role in DoS**                                |
|---------------------|------------------------------------------------|
| NTP — UDP 123       | Amplification via 'monlist' feature            |
| DNS — UDP 53        | Amplification (small query, big response)      |
| SSDP — UDP 1900     | Amplification (IoT/UPnP devices)               |
| ICMP (no port)      | Ping/Smurf/ICMP floods                         |
| TCP (SYN)           | SYN flood abuses the 3-way handshake           |
| HTTP 80 / HTTPS 443 | App-layer floods (Slowloris, GET/POST, HTTP/2) |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Why UDP for amplification?</strong></p>
<p><strong>•</strong> UDP is CONNECTIONLESS (stateless) — no handshake — so the source IP is easy to spoof and the server just replies to whoever asked (the victim). TCP's handshake makes spoofing harder.</p></td>
</tr>
</tbody>
</table>

# **9) Tools (attack & detection)**

| **Tool**                        | **Purpose**                                                                                                | **Exam takeaway**                                                                   |
|---------------------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| **HULK**                        | HTTP Unbearable Load King — generates HTTP GET floods designed to bypass caching and stress the web server | **Application-layer HTTP flood**                                                    |
| **LOIC / HOIC**                 | Generate large volumes of network/HTTP traffic                                                             | **Volumetric DoS/DDoS tools**; commonly associated with hacktivist activity         |
| **Slowloris**                   | Opens many HTTP connections and keeps them alive by sending data very slowly                               | **Slow HTTP / low-bandwidth application-layer DoS**                                 |
| **ISB / UltraDDoS-v2 / UFONet** | Generate different types of DoS/DDoS traffic                                                               | **DoS/DDoS attack toolkits**                                                        |
| **hping3**                      | Creates/customizes TCP/IP packets; can be used for SYN floods and other packet-based attacks               | **Custom packet crafting / network-layer attacks**                                  |
| **Blumira**                     | Security monitoring/detection platform                                                                     | **Detection / defensive monitoring — NOT an attack tool** |
| **Honeypot**                    | Decoy system designed to attract and detect suspicious activity                                            | **Defense / detection — NOT an attack tool**              |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Lab command (authorized only)</strong></p>
<p><strong>•</strong> SYN flood: hping3 -S &lt;victim&gt; -a &lt;spoofed-IP&gt; -p 80 --flood</p>
<p><strong>•</strong> NTP monlist check: nmap -sU -p 123 --script=ntp-monlist &lt;target&gt;</p></td>
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
<p><strong>•</strong> A web server suddenly has thousands of half-open connections; each shows a SYN received but no final ACK.</p>
<p><strong>•</strong> Source IPs look random and fake (spoofed).</p>
<p><strong>•</strong> The connection table fills up, so real users get 'connection refused'.</p>
<p><strong>•</strong> Diagnosis: SYN flood (protocol attack on the TCP 3-way handshake).</p>
<p><strong>•</strong> Fix: SYN cookies, rate limiting, and a firewall/anti-DDoS service (e.g., cloud scrubbing).</p></td>
</tr>
</tbody>
</table>

# **11) Detection & countermeasures**

## **Detection techniques (from your notes)**

| **Detection technique**               | **Simple meaning**                       | **What it looks for**                            | **Easy example**                                                 | **Exam takeaway**                                |
|---------------------------------------|------------------------------------------|--------------------------------------------------|------------------------------------------------------------------|--------------------------------------------------|
| **Activity profiling**                | Learn what **normal traffic** looks like | Traffic above the normal baseline                | Normal = 100 Mbps, suddenly = 1 Gbps                             | **Baseline → detect abnormal spike**             |
| **Sequential change-point detection** | Look for a **sudden change over time**   | A significant change in traffic from an IP/flow  | IP normally sends 10 requests/sec → suddenly 5,000               | **Sudden change → possible attack**              |
| **Wavelet-based signal analysis**     | Analyze traffic as a **signal**          | Changes in frequency/patterns of network traffic | Detect unusual traffic patterns hidden in normal-looking traffic | **Signal/frequency analysis → detect anomalies** |

## **Prevention / mitigation**

| **Technique**                                | **Simple meaning**                                                                  | **Protects against / purpose**                    | **Exam takeaway**                                     |
|----------------------------------------------|-------------------------------------------------------------------------------------|---------------------------------------------------|-------------------------------------------------------|
| **SYN cookies**                              | Don't keep a connection state until the client proves it can complete the handshake | **SYN flood**                                     | **SYN flood → SYN cookies**                           |
| **Increase connection backlog**              | Allow more pending connections to wait                                              | Helps absorb bursts of **SYN connections**        | **More backlog = more pending connections tolerated** |
| **Rate limiting**                            | Limit how much traffic/requests one source can send                                 | Floods / excessive requests                       | **Too many requests → limit the rate**                |
| **Traffic filtering**                        | Block unwanted/malicious traffic                                                    | DoS/DDoS and malicious traffic                    | **Filter bad traffic**                                |
| **Blackhole routing**                        | Route attack traffic to a null route so it is discarded                             | Large DDoS floods                                 | **Blackhole = throw traffic away**                    |
| **Sinkhole routing**                         | Redirect suspicious traffic to a controlled destination for analysis/handling       | Malicious/suspicious traffic                      | **Sinkhole = redirect traffic**                       |
| **Ingress filtering**                        | Filter traffic **entering** a network                                               | **IP spoofing**                                   | **Ingress = coming IN**                               |
| **Egress filtering**                         | Filter traffic **leaving** a network                                                | Prevent your network from sending spoofed traffic | **Egress = going OUT**                                |
| **Disable NTP monlist**                      | Disable the vulnerable NTP monitoring feature that can be abused for amplification  | **NTP amplification DDoS**                        | **NTP monlist → amplification**                       |
| **Secure open DNS resolvers**                | Prevent unauthorized recursive DNS queries                                          | **DNS amplification**                             | **Open resolver → DNS amplification**                 |
| **Cloud DDoS protection / scrubbing center** | Provider filters and absorbs attack traffic before it reaches your network          | Large-scale **DDoS**                              | **Scrubbing = clean traffic upstream**                |
| **Load balancing**                           | Distribute traffic across multiple servers                                          | Reduces overload on one server                    | **Spread the load**                                   |
| **CDN**                                      | Distribute content through many edge servers                                        | Helps absorb/distribute high traffic              | **CDN = traffic distributed closer to users**         |

# **12) Related concepts**

| **Concept**            | **One-line meaning**                                   |
|------------------------|--------------------------------------------------------|
| Botnet / zombie        | Network of infected machines used to attack            |
| C2 (Command & Control) | Server the attacker uses to control the botnet         |
| Multi-vector DDoS      | Combines **volumetric + protocol + app-layer** at once |
| Ransom DDoS (RDDoS)    | Attack (or threat) demanding payment to stop           |
| TCP SACK Panic         | Linux-only crash via malformed SACK/MSS packets        |

# **13) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> DoS = one source; DDoS = many sources (botnet). Read the source count.</p>
<p><strong>•</strong> SYN flood = protocol attack (half-open connections), NOT volumetric bandwidth.</p>
<p><strong>•</strong> Smurf = ICMP + broadcast + spoofed victim IP. Fraggle = same idea but UDP.</p>
<p><strong>•</strong> Amplification abuses STATELESS UDP (NTP/DNS/SSDP) — small request, big reply.</p>
<p><strong>•</strong> Slowloris is LOW-bandwidth (app-layer), the opposite of a volumetric flood.</p>
<p><strong>•</strong> Permanent DoS (phlashing) damages HARDWARE — not just downtime.</p>
<p><strong>•</strong> DoS targets AVAILABILITY (the 'A' in CIA), not confidentiality.</p></td>
</tr>
</tbody>
</table>

# **14) Mnemonics**

- **Three categories:** 'Volume Prevents Access' → Volumetric, Protocol, Application-layer.

- **Amplification services:** 'Never Do SSDP' → NTP, DNS, SSDP (stateless UDP).

- **SYN flood:** 'Half-open, never closed' → fills the connection table.

- **Smurf:** 'Spoof + Broadcast + ICMP.'

- **Target:** 'DoS kills the A' → Availability in CIA.

# **15) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> DoS = 1 source, DDoS = many (botnet). Goal = availability (downtime), not theft.</p>
<p><strong>•</strong> 3 categories: Volumetric (bps), Protocol (state), Application-layer (rps).</p>
<p><strong>•</strong> SYN flood = protocol; abuses TCP 3-way handshake; half-open connections. Fix: SYN cookies.</p>
<p><strong>•</strong> Smurf = spoofed victim IP + ICMP echo to broadcast. Fraggle = UDP version.</p>
<p><strong>•</strong> Amplification/reflection = stateless UDP: NTP (123), DNS (53), SSDP (1900). Small request → big reply.</p>
<p><strong>•</strong> Ping of Death = oversized packet. Slowloris = low-bandwidth app-layer.</p>
<p><strong>•</strong> Permanent DoS (phlashing) = bricks hardware via fake firmware.</p>
<p><strong>•</strong> HTTP/2 Rapid Reset = 2023 stream-multiplexing DDoS.</p>
<p><strong>•</strong> Botnet controlled via C2. Zombies = infected bots.</p>
<p><strong>•</strong> Detection: activity profiling, change-point detection, wavelet analysis.</p>
<p><strong>•</strong> Mitigation: SYN cookies, rate limiting, anti-spoof filtering, cloud scrubbing, CDN/load balancing.</p></td>
</tr>
</tbody>
</table>

*End of Module 10 — Denial of Service. Next up: quiz this module, or move to Module 11 (Session Hijacking).*
