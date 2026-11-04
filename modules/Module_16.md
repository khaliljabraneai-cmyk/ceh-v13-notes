**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 16 — Hacking Wireless Networks**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Project documents (16.1–16.5 study sheets, Boson notes) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

**Wireless hacking** means finding and exploiting **weaknesses in Wi-Fi networks** to gain unauthorized access. A wireless network uses radio waves (not cables) at the physical layer, so the signal is a **BROADCAST** anyone nearby can hear.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The open-window picture</strong></p>
<p><strong>•</strong> A wired network is like talking inside a closed room. Wi-Fi is like shouting out of an open window — everyone within range hears you.</p>
<p><strong>• That's why ENCRYPTION is mandatory on wireless</strong>: it scrambles the shout so only the right listener understands.</p>
<p><strong>•</strong> Core idea: wireless = broadcast, not point-to-point.</p></td>
</tr>
</tbody>
</table>

# **2) Key terms (high yield)**

| Term / Full form                     | Simple meaning                                             | Exam trap / Remember                    | Simple example                                 |
|--------------------------------------|------------------------------------------------------------|-----------------------------------------|------------------------------------------------|
| SSID — Service Set Identifier        | Wi-Fi network name                                         | SSID ≠ BSSID                            | Office_WiFi                                    |
| BSSID — Basic Service Set Identifier | Identifies a specific AP (usually its radio MAC address)   | Identifies the AP, not the network name | AA:BB:CC:11:22:33                              |
| AP — Access Point                    | Connects wireless devices to a wired LAN                   | Provides wireless network access        | Office Wi-Fi router                            |
| Association                          | Process of a device connecting to an AP                    | Association ≠ authentication            | Your laptop joins Office_WiFi                  |
| NIC — Network Interface Card         | Hardware that connects a device to a network               | A Wi-Fi adapter is a wireless NIC       | Laptop Wi-Fi adapter                           |
| Monitor mode                         | Allows a compatible Wi-Fi NIC to capture raw 802.11 frames | Monitor mode ≠ promiscuous mode         | Capturing Wi-Fi frames for authorized analysis |
| LAN — Local Area Network             | Network covering a limited area                            | Can connect wired and wireless devices  | Company office network                         |
| MAC — Media Access Control           | Link-layer addressing used by network interfaces           | A BSSID commonly uses a MAC address     | 00:1A:2B:3C:4D:5E                              |
| Hotspot                              | Provides Wi-Fi network access                              | Can be public or private                | Café Wi-Fi or phone tethering                  |

# 

# **3) Wireless encryption standards (the centerpiece)**

The single most-tested table in this module. Memorize the cipher and key management for each.

**Wireless Encryption Standards (CEH v13)**

| **Standard**                    | **Cipher**                       | **Key Management / Handshake** | **Security**          | **Simple Understanding**                                                           |
|---------------------------------|----------------------------------|--------------------------------|-----------------------|------------------------------------------------------------------------------------|
| WEP (Wired Equivalent Privacy)  | RC4 + 24-bit IV                  | Usually a static key           | Weak / obsolete       | Old Wi-Fi security. Reuses keys in ways that make it easier to crack.              |
| WPA (Wi-Fi Protected Access)    | RC4 + TKIP                       | TKIP improves key management   | Outdated / deprecated | An improvement over WEP, but still uses old encryption technology.                 |
| WPA2 (Wi-Fi Protected Access 2) | AES (typically)                  | CCMP + 4-way handshake         | Strong / widely used  | Uses strong encryption. The handshake helps devices establish session keys.        |
| WPA3 (Wi-Fi Protected Access 3) | AES-based encryption (typically) | SAE in Personal mode           | Stronger / newer      | Improves password-based authentication and makes offline password guessing harder. |

## **What to remember per standard**

- **WEP:** RC4 + small 24-bit IV. Weakness = IV reuse → cracked in minutes. 'Weak Encryption Protocol.'

- **WPA:** added TKIP + MIC but still uses RC4. 'WEP with patches.'

- **WPA2:** AES-CCMP, 4-way handshake. Strong crypto, but weak PASSWORDS + KRACK are the risks.

- **WPA3:** SAE (Dragonfly) handshake stops OFFLINE dictionary attacks; forward secrecy. Vuln: Dragonblood.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam traps (encryption)</strong></p>
<p><strong>•</strong> Weakest = WEP (IV reuse). Uses TKIP = WPA. Uses AES = WPA2/WPA3. Uses SAE = WPA3.</p>
<p><strong>•</strong> Ladder: WEP → WPA → WPA2 → WPA3 = Weak → Better → Strong → Strongest.</p></td>
</tr>
</tbody>
</table>

# **4) Wireless threats & attacks (high yield)**

| **Attack name**                                                    | **Simple explanation: How it works and its objective**                                                                                                               | **Goal**                            |
|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------|
| Rogue AP (Rogue Access Point)                                      | An unauthorized person connects a Wi-Fi access point to a company network. Objective: Create an unauthorized way into the network.                                   | Backdoor access                     |
| Evil Twin                                                          | An attacker creates a fake Wi-Fi network with the same name as a legitimate one to trick users into connecting. Objective: Steal passwords or spy on users' traffic. | Credential theft / MITM             |
| Deauthentication                                                   | An attacker sends fake Wi-Fi messages that force devices to disconnect from the access point. Objective: Interrupt the connection or force devices to reconnect.     | Force reconnect → capture handshake |
| Disassociation                                                     | An attacker sends fake messages that break the connection between a device and its access point. Objective: Disconnect users and disrupt Wi-Fi access.               | Disruption                          |
| Replay Attack                                                      | An attacker records wireless messages and sends them again later. Objective: Repeat an action or exploit weak security protections.                                  | Auth bypass (esp. WEP)              |
| WEP Cracking                                                       | An attacker captures lots of WEP-encrypted Wi-Fi traffic and analyzes weaknesses in the encryption. Objective: Discover the Wi-Fi key.                               | Recover WEP key                     |
| KRACK (Key Reinstallation Attack)                                  | An attacker exploits a weakness in how a vulnerable device handles WPA2 encryption keys during the handshake. Objective: Potentially decrypt or replay some traffic. | Decrypt traffic / replay            |
| Packet Injection                                                   | An attacker inserts specially crafted packets into wireless traffic. Objective: Manipulate communication or help perform other attacks, such as WEP cracking.        | Speed up WEP cracking               |
| Jamming (التشويش) | An attacker creates radio interference that prevents Wi-Fi devices from communicating properly. Objective: Make Wi-Fi unavailable (Denial of Service).               | Denial of service                   |
| MITM (Man-in-the-Middle)                                           | An attacker places themselves between a user and the network to intercept communication. Objective: Spy on or modify traffic, or steal information.                  | Eavesdrop / alter                   |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Rogue AP vs Evil Twin (exam favorite)</strong></p>
<p><strong>•</strong> Rogue AP = UNAUTHORIZED (can be an employee's own AP); does NOT mimic a signal.</p>
<p><strong>•</strong> Evil Twin = FAKE AP that mimics a legit SSID to trick victims. 'Rogue = unauthorized, Evil Twin = fake.'</p>
<p><strong>•</strong> Deauth vs Disassoc: deauth ends AUTHENTICATION; disassoc ends ASSOCIATION.</p>
<p><strong>•</strong> Deauth does NOT break encryption — it just forces a reconnect so you can capture the handshake.</p>
<p>Deauthentication = authenticate + associate again; Disassociation = associate again.</p>
<p>Authentication → Association → Connected</p></td>
</tr>
</tbody>
</table>

# **5) Wireless hacking methodology (5 phases)**

| \#  | Phase                  | What happens / tools                                                                                                                                                                                                                            |
|-----|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1   | Reconnaissance         | Discover nearby Wi-Fi networks and identify their SSID (name), BSSID (AP MAC address), channel, and encryption. **Tools: Kismet, NetStumbler, inSSIDer, airodump-ng.**                                                                          |
| 2   | Scanning + Enumeration | Investigate discovered networks, identify access points and clients, and gather more detailed information, including WPS availability. **Tools: airmon-ng (monitor mode), airodump-ng (wireless discovery and capture), WASH (WPS discovery).** |
| 3   | Gaining Access         | Attempt to exploit weak wireless security. Examples: WEP cracking, WPA/WPA2 handshake attacks, Evil Twin, WPS PIN attacks.                                                                                                                      |
| 4   | Maintaining Access     | Attempt to preserve access to a compromised network. Examples/tools: rogue or persistent access points, hostapd, MAC address changes using macchanger.                                                                                          |
| 5   | Covering Tracks        | Attempt to conceal activity by altering logs or disguising a device's MAC address. Defenders should preserve logs and investigate suspicious changes.                                                                                           |

## **The Aircrack-ng workflow (memorize the order)**

airmon-ng start wlan0 \# 1. enable monitor mode

airodump-ng wlan0mon \# 2. scan / find target + capture

aireplay-ng --deauth 10 ... \# 3. deauth to force the 4-way handshake

aircrack-ng capture.cap -w wordlist \# 4. crack the captured handshake

| **Command**                             | **Simple explanation**                                                                    |
|-----------------------------------------|-------------------------------------------------------------------------------------------|
| **airmon-ng start wlan0**               | Enable monitor mode to capture Wi-Fi traffic.                                             |
| **airodump-ng wlan0mon**                | Scan nearby Wi-Fi networks and capture packets.                                           |
| **aireplay-ng --deauth 10 ...**         | Disconnect a client to make it reconnect and help capture the 4-way handshake.            |
| **aircrack-ng capture.cap -w wordlist** | Test passwords from a wordlist against the captured handshake to find the Wi-Fi password. |

- **WEP vs WPA2 cracking:** WEP = capture many IVs then crack. WPA2 = capture the 4-way HANDSHAKE (via deauth) then crack the PSK OFFLINE with a wordlist.

# **6) Tools (master table)**

| **Tool**               | **Function**                                                                                                      |
|------------------------|-------------------------------------------------------------------------------------------------------------------|
| airmon-ng              | Enable monitor mode so a compatible Wi-Fi adapter can capture wireless traffic.                                   |
| airodump-ng            | Find nearby Wi-Fi networks and capture wireless packets.                                                          |
| aireplay-ng            | Inject wireless packets; can send deauthentication frames to disconnect clients during authorized testing.        |
| aircrack-ng            | Test password guesses to crack WEP keys or WPA/WPA2 passwords.                                                    |
| Kismet                 | Passively monitor Wi-Fi networks and detect nearby wireless devices without actively transmitting attack packets. |
| Reaver / Bully         | Test WPS PIN security by attempting to guess the PIN on vulnerable access points.                                 |
| Wash                   | Identify nearby Wi-Fi access points that have WPS enabled.                                                        |
| NetStumbler / inSSIDer | Discover nearby Wi-Fi networks and analyze signal strength, channels, and network information.                    |
| macchanger             | Change or spoof a device's MAC address.                                                                           |

- **With WPS, you can instead press the WPS button on the router and connect a compatible device without manually typing the Wi-Fi password.**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Tool → attack mapping</strong></p>
<p><strong>•</strong> airodump-ng = recon ·</p>
<p>aireplay-ng = deauth ·</p>
<p>aircrack-ng = key cracking ·</p>
<p>reaver = WPS brute force ·</p>
<p>Kismet = passive sniffing.</p></td>
</tr>
</tbody>
</table>

# **7) Important concepts (don't mix up)**

| **Concept**                  | **Meaning**                                                                                                                                                                                                           |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Monitor mode                 | Allows a compatible Wi-Fi adapter to capture wireless radio frames, including traffic not addressed to your device. Used for Wi-Fi sniffing.                                                                          |
| Promiscuous mode             | Allows a network card to receive Ethernet frames beyond those addressed to its own MAC address, subject to network and hardware limitations. Commonly associated with wired sniffing.                                 |
| 4-way handshake              | WPA/WPA2 process in which the access point and client exchange messages to confirm they have the necessary key material and establish session keys. Captured handshakes can be used to test password guesses offline. |
| SAE (Dragonfly)              | WPA3-Personal authentication method that helps prevent attackers from validating password guesses offline using a captured exchange.                                                                                  |
| PMK (Pairwise Master Key)    | Master key material used to derive keys for a Wi-Fi connection.                                                                                                                                                       |
| PTK (Pairwise Transient Key) | Session key material used to protect unicast traffic between a client and an access point.                                                                                                                            |

# **8) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> A tester wants the WPA2 password of a cafe AP (authorized test).</p>
<p><strong>•</strong> airmon-ng puts the card in monitor mode; airodump-ng finds the AP and a connected client.</p>
<p><strong>•</strong> aireplay-ng sends deauth frames → the client reconnects → the 4-way HANDSHAKE is captured.</p>
<p><strong>•</strong> aircrack-ng runs the capture against a wordlist offline → weak password cracked.</p>
<p><strong>•</strong> Fix: long random passphrase (or WPA3-SAE), disable WPS, enable WIPS + 802.11w.</p>
<p>Step</p>
<p>Simple explanation</p>
<p>1. airmon-ng</p>
<p>Enable monitor mode so the Wi-Fi adapter can capture wireless traffic.</p>
<p>2. airodump-ng</p>
<p>Find the café's Wi-Fi network (AP) and identify connected clients.</p>
<p>3. aireplay-ng</p>
<p>Send deauthentication frames to disconnect a client and trigger reconnection, potentially allowing the tester to capture the WPA2 4-way handshake.</p>
<p>4. aircrack-ng</p>
<p>Test passwords from a wordlist against the captured handshake offline. A weak password may be discovered if it appears in the list.</p>
<p>5. Fix: Strong passphrase</p>
<p>Use a long, random Wi-Fi password that is difficult to guess.</p>
<p>6. Fix: WPA3-SAE</p>
<p>Use WPA3-Personal with SAE where supported to improve protection against offline password guessing.</p>
<p>7. Fix: Disable WPS</p>
<p>Turn off Wi-Fi Protected Setup to reduce the risk of WPS PIN attacks.</p>
<p>8. Fix: WIPS + 802.11w</p>
<p>Use a Wireless Intrusion Prevention System (WIPS) to detect suspicious activity and enable 802.11w (Protected Management Frames) to protect supported management frames, including deauthentication frames.</p></td>
</tr>
</tbody>
</table>

# **9) Countermeasures**

- **Encryption:** use WPA3 (WPA2-AES minimum). Disable WEP/WPA entirely.

- **Authentication:** strong passphrase; WPA2/WPA3-Enterprise (RADIUS) \> Personal; use certificates.

- **Disable WPS:** it is PIN brute-forceable.

- **WIDS/WIPS:** detect (IDS) and actively stop (IPS) rogue APs, Evil Twins, deauth.

- **802.11w:** Management Frame Protection — stops deauth/disassoc attacks.

- Also: network segmentation (guest VLAN), VPN over Wi-Fi, patch firmware, physical security.

## **Attack → countermeasure mapping (exam favorite)**

| **Attack**           | **Countermeasure** |
|----------------------|--------------------|
| Evil Twin / Rogue AP | WIDS/WIPS          |
| Deauth               | 802.11w            |
| WEP cracking         | WPA3               |
| MITM                 | Strong encryption  |
| Jamming              | Spectrum analysis  |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Weak controls that are NOT real security</strong></p>
<p><strong>•</strong> Hidden SSID = deterrent only, not protection.</p>
<p><strong>•</strong> MAC filtering = 'speed bump' — MACs are easily spoofed.</p></td>
</tr>
</tbody>
</table>

# **10) Standards to remember**

| Standard / Technology | Simple explanation                                                                                                                                                              |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 802.11                | The IEEE family of standards that defines how Wi-Fi wireless networks communicate.                                                                                              |
| 802.11i               | The security standard that introduced the foundation for **WPA2**, including stronger authentication and encryption.                                                            |
| 802.11w               | Protects certain Wi-Fi management frames, helping defend against forged deauthentication and disassociation attacks. Now incorporated into newer Wi-Fi security specifications. |
| OFDM                  | A wireless transmission technique that divides data across multiple subcarriers to transmit information efficiently and reliably.                                               |
| MIMO                  | Uses multiple antennas to transmit and receive data, improving Wi-Fi speed, capacity, and reliability.                                                                          |

# **11) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> WEP = weakest (IV reuse). WPA = TKIP. WPA2 = AES-CCMP. WPA3 = SAE.</p>
<p><strong>•</strong> Monitor mode (wireless) ≠ promiscuous mode (wired).</p>
<p><strong>•</strong> Deauth does NOT break encryption; it forces a reconnect to capture the handshake.</p>
<p><strong>•</strong> Rogue AP = unauthorized (maybe internal); Evil Twin = fake AP mimicking a real SSID.</p>
<p><strong>•</strong> WPA2 is strong crypto but falls to weak PASSWORDS and KRACK.</p>
<p><strong>•</strong> WPA3-SAE defeats OFFLINE dictionary attacks (its headline benefit).</p>
<p><strong>•</strong> Hidden SSID and MAC filtering are NOT real security.</p>
<p><strong>•</strong> WPS is a weak point — disable it (Reaver brute-forces the PIN).</p></td>
</tr>
</tbody>
</table>

# **12) Mnemonics**

- **Encryption ladder:** 'Weak, Better, Strong, Strongest' → WEP, WPA, WPA2, WPA3.

- **Ciphers:** 'RC4, RC4, AES, AES' and 'Static, TKIP, CCMP, SAE.'

- **Methodology:** 'Recon, Scan, Access, Persist, Hide.'

- **Aircrack flow:** 'airmon → airodump → aireplay → aircrack.'

- **Defense:** 'IDS sees, IPS stops' · '11w stops deauth.'

# **13) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Wireless = broadcast; encryption mandatory. SSID = name, BSSID = AP MAC.</p>
<p><strong>•</strong> WEP: RC4 + 24-bit IV, static key, IV reuse = cracked fast.</p>
<p><strong>•</strong> WPA: RC4 + TKIP (patched WEP). WPA2: AES-CCMP + 4-way handshake. WPA3: SAE (Dragonfly) + forward secrecy.</p>
<p><strong>•</strong> WPA2 risks: weak passphrase + KRACK (4-way handshake). WPA3 risk: Dragonblood.</p>
<p><strong>•</strong> Attacks: Rogue AP (unauthorized), Evil Twin (fake), Deauth (force reconnect), KRACK, Jamming (DoS), MITM.</p>
<p><strong>•</strong> Methodology: Recon (airodump/Kismet) → Scan (airmon/wash) → Access (crack/Evil Twin/WPS) → Maintain → Cover.</p>
<p><strong>•</strong> Aircrack order: airmon-ng → airodump-ng → aireplay-ng (deauth) → aircrack-ng.</p>
<p><strong>•</strong> WEP crack = collect IVs; WPA2 crack = capture 4-way handshake, crack PSK offline.</p>
<p><strong>•</strong> WPS brute force = Reaver/Bully; detect with Wash. Disable WPS.</p>
<p><strong>•</strong> Monitor mode captures Wi-Fi (not promiscuous). 802.11w stops deauth.</p>
<p><strong>•</strong> Defense: WPA3, disable WPS, WIDS/WIPS, strong passphrase, segment, VPN, patch.</p>
<p><strong>•</strong> Not security: hidden SSID, MAC filtering.</p></td>
</tr>
</tbody>
</table>

*End of Module 16 — Hacking Wireless Networks. Next up: quiz this module, or move to Module 17 (Hacking Mobile Platforms).*

**WPA2 uses AES with CCMP and a 4-way handshake**
