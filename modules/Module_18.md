**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 18 — IoT & OT Hacking**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Project documents (18.1–18.5 study sheets) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

**IoT (Internet of Things)** = everyday physical devices (cameras, sensors, smart home gear) connected to the internet. **OT (Operational Technology)** = systems that run industrial/physical processes (factories, power grids) using PLCs, SCADA and HMIs. Hacking them means exploiting their weak security to control devices or even cause physical damage.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The factory picture</strong></p>
<p><strong>•</strong> IoT = the smart gadgets (a camera, a thermostat) — cheap, always-on, rarely patched.</p>
<p><strong>•</strong> OT = the machines that run a plant (valves, turbines) controlled by PLCs — old, critical, and built for uptime not security.</p>
<p><strong>•</strong> The scary part: an OT hack isn't just data loss — it can break physical equipment (Stuxnet destroyed centrifuges).</p>
<p><mark><strong>PLCs = Programmable Logic Controllers</strong> (A PLC is an industrial computer that automatically controls machines and industrial processes)</mark></p></td>
</tr>
</tbody>
</table>

# **2) Why IoT & OT are so vulnerable**

| IoT weaknesses                                                 | OT weaknesses                                                                    |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Default/hardcoded credentials — devices keep factory passwords | Legacy systems — old equipment and software                                      |
| Weak or no encryption — data can be intercepted                | No authentication on some industrial protocols                                   |
| Insecure web interfaces — vulnerable device dashboards         | Flat networks — IT and industrial systems may share a network                    |
| Rarely patched — limited resources or vendor support           | Downtime unacceptable — patching may stop production                             |
| Physically accessible — attackers can reach devices            | Safety prioritized over security — changes must not endanger people or equipment |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hooks</strong></p>
<p><strong>•</strong> IoT: 'Cheap, old, exposed.' OT: 'Old + critical = vulnerable.'</p>
<p><strong>•</strong> Most OT protocols have NO native security. <mark>(Some OT protocols may not verify who sent the command. An unauthorized device with network access might send a command that the PLC accepts.)</mark></p></td>
</tr>
</tbody>
</table>

# 

# **3) IoT essentials (know these)**

## **IoT application protocols**

| **Protocol (abbreviation + full name)**               | **Main function / simple meaning**                                                                  | **Simple example**                                                 | **Exam keyword**           |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|----------------------------|
| **MQTT — Message Queuing Telemetry Transport**        | Sends and distributes messages through a central **broke**r using **publish/subscribe**.            | A temperature sensor sends readings to a smart-home app.           | Publish/Subscribe + Broker |
| **CoAP — Constrained Application Protocol**           | Allows small devices to request data and control resources, similar to **HTTP; commonly uses UDP.** | An app checks a smart light's status or tells it to turn on.       | Lightweight + UDP          |
| **AMQP — Advanced Message Queuing Protocol**          | Exchanges messages between applications using **messaging queues**.                                 | A factory system queues orders for another application to process. | Message Queuing            |
| **XMPP — Extensible Messaging and Presence Protocol** | **Exchanges messages** and presence information, such as online/offline status.                     | A device sends a message indicating that it is online.             | Messaging + XML            |

## **Communication ranges (how far devices communicate)**

- **Short-range:** Bluetooth LE, NFC, RFID, ZigBee, Z-Wave.

- **Long-range low-power**: LoRaWAN, Sigfox, NB-IoT.

- **Models (Communication models — how data travels) :** Device-to-Device, Device-to-Cloud, Device-to-Gateway, Back-end data sharing (D2D, D2C, D2G).

# **4) IoT threats & attacks (high yield)**

 **Need to remember: 3 categories: Device → Network → Botnet.**

- **Device = What is attacked? The device.**

- **Network = What is attacked? The communication.**

- **Botnet = What is controlled? A group of infected devices.**

## **Device-level**

| Attack              | Simple explanation                                              | Exam keyword        |
|---------------------|-----------------------------------------------------------------|---------------------|
| Default credentials | Use factory username/password to log in.                        | Default password    |
| Firmware tampering  | Modify device software to install a backdoor.                   | Modified firmware   |
| JTAG access         | Use a hardware debugging port to access memory or firmware.     | Hardware debug      |
| UART access         | Use a serial console to access the device, potentially as root. | Serial console      |
| Chip-off            | Physically remove a memory chip to extract its data.            | Physical extraction |

## 

## 

## **Network & protocol**

- **MQTT attacks:** subscribe without auth, publish fake messages, broker takeover. 'No-auth MQTT = open mic.'

- **CoAP attacks:** amplification, replay, spoofing (because it runs over UDP).

- Also: MITM, DNS spoofing, jamming (ZigBee/Bluetooth).

| **Attack**         | **Simple explanation**                                                      | **Remember**                 |
|--------------------|-----------------------------------------------------------------------------|------------------------------|
| MQTT attack        | Access an insecure broker to read or publish fake messages.                 | No authentication = open mic |
| CoAP amplification | Exploit UDP-based communication to generate larger traffic toward a victim. | Amplification                |
| Replay attack      | Capture a legitimate message and send it again.                             | Reuse captured data          |
| Spoofing           | **Pretend to be another** device or sender.                                 | Fake identity                |
| MITM               | Intercept communication between two devices.                                | Intercept                    |
| DNS spoofing       | Provide a fake DNS response to redirect traffic.                            | Fake IP resolution           |
| Jamming            | Interfere with wireless signals to disrupt communication.                   | Block radio signals          |

## **Botnets (exam favorite)**

- **Mirai:** infects IoT via DEFAULT credentials, scans Telnet/SSH, builds a botnet for massive DDoS. 'Mirai = IoT DDoS.'

- Others: Reaper, Hajime, Bashlite, Mozi. Flow: Scan → Infect → Control → Flood.

**Mirai:** Malware that infects vulnerable IoT devices, often by trying default credentials, and uses them together as a botnet to launch DDoS attacks. Other IoT botnet names to recognize**: Reaper, Hajime, Bashlite, Mozi.**

**How a botnet works**

<img src="media/image1.png" style="width:6.17393in;height:2.73625in" />

# **5) IoT hacking tools & phases**

- **Phases:** Reconnaissance → Scanning → Gaining access → Maintaining access → Covering tracks.

| **Tool**         | **Very easy meaning**                               | **Daily-life example**                                                      | **Exam keyword**   |
|------------------|-----------------------------------------------------|-----------------------------------------------------------------------------|--------------------|
| **Shodan**       | Finds devices connected to the internet.            | Finding security cameras that are visible on the internet.                  | Find devices       |
| **Censys**       | Finds internet-connected systems and websites.      | Making a list of computers and websites exposed to the internet.            | Discover systems   |
| **Nmap**         | Checks which ports (doors) are open.                | Checking which doors are open on your home router.                          | Open ports         |
| **RouterSploit** | Tests routers for security weaknesses.              | Checking whether your home router has a known security flaw.                | Test routers       |
| **Metasploit**   | Tests whether a security weakness can be exploited. | Safely testing whether a known weakness lets someone enter a test computer. | Exploit weaknesses |
| **Exploit-DB**   | A library of known hacking exploits.                | Looking up known weaknesses and their exploits in a technical library.      | Find exploits      |

# **6) OT / ICS essentials (high yield)**

## **The OT components**

- **OT (Operational Technology)** = The big category: technology that monitors or controls physical machines and processes.

- **ICS (Industrial Control System)** = Systems within OT used to control industrial processes.

| Component                                            | Very easy meaning                                                  | Daily-life example                                                 |
|------------------------------------------------------|--------------------------------------------------------------------|--------------------------------------------------------------------|
| **PLC (Programmable Logic Controller)**              | Controls a machine automatically.                                  | Turns a water pump ON when needed.                                 |
| **RTU (Remote Terminal Unit)**                       | Collects information and controls equipment in a remote location.  | Sends water tank level information from a distant site.            |
| **HMI (Human-Machine Interface)**                    | Screen that lets a person monitor and operate machines.            | Operator sees the tank level and presses a button to start a pump. |
| **SCADA (Supervisory Control and Data Acquisition)** | System that monitors and supervises many machines or remote sites. | Control center monitors water tanks and pumps across a city.       |

**OT** is the big family that controls physical machines; **ICS** is the industrial control system, where the **HMI** lets the operator give commands, the **PLC** controls the machine, the **RTU** handles remote equipment, and **SCADA** supervises the whole process across multiple locations.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Quick recall</strong></p>
<p><strong>•</strong> 'SCADA supervises, PLC controls, HMI shows.'</p></td>
</tr>
</tbody>
</table>

## 

## **Purdue Model (levels)**

**Purdue Model**: A model that organizes an industrial network into levels, from physical machines (Level 0) to the company's business network (Level 5), helping separate and secure them.

| **Level** | **Very easy meaning** | **Daily-life example**                             |
|-----------|-----------------------|----------------------------------------------------|
| Level 0   | Physical process      | Water flowing through pipes                        |
| Level 1   | Sensors and actuators | Sensor measures water level; valve opens or closes |
| Level 2   | Controls machines     | PLC controls the pump                              |
| Level 3   | Supervises operations | SCADA monitors the plant                           |
| Level 4   | Site/business IT      | Office systems, production planning                |
| Level 5   | Enterprise network    | Company-wide business network and services         |

- **Memory hook:** '0 = process, 5 = business.' Keep IT (4-5) separated from OT (0-3) via a DMZ.

<!-- -->

- **A DMZ (Demilitarized Zone)** is a controlled network area placed between industrial OT and business IT to help prevent direct, unrestricted communication.

## **OT protocols (insecure by default)**

- **Important exam note**: These protocols are not all equally insecure in every configuration. Traditional Modbus lacks built-in authentication; security for DNP3 and the other protocols depends on the protocol version, security extensions, and deployment.

| Protocol     | Simple meaning                                       | Daily-life example                                                | Exam memory hook             |
|--------------|------------------------------------------------------|-------------------------------------------------------------------|------------------------------|
| **Modbus**   | Lets industrial machines exchange data and commands. | A computer tells a water pump to start.                           | No authentication by default |
| **DNP3**     | Communicates with remote equipment in utilities.     | A control center monitors a distant electricity substation.       | Power utilities              |
| **PROFIBUS** | Connects industrial equipment on a factory network.  | A controller communicates with factory sensors.                   | Industrial fieldbus          |
| **PROFINET** | Uses Ethernet for industrial communication.          | Factory machines communicate over an industrial Ethernet network. | Industrial Ethernet          |
| **BACnet**   | Lets building systems communicate.                   | A building system controls air conditioning and heating (HVAC).   | Building automation          |

# **7) OT attacks & famous incidents**

- **Attack types:** unauthorized command execution, process manipulation (lying sensors), OT DoS (shutdown), MITM (command modification), ransomware.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 79%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Incident</strong></th>
<th><strong>Why it matters</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Stuxnet</td>
<td><p>Targeted PLCs, sabotaged Iranian centrifuges using zero-days — the first cyber-WEAPON</p>
<p>Malware sabotaged Iranian nuclear centrifuges by manipulating industrial controllers.</p></td>
</tr>
<tr class="even">
<td>Triton / Trisis</td>
<td><p>Targeted SAFETY systems — potential lethal impact</p>
<p>Malware targeted industrial safety systems designed to protect people and equipment.</p></td>
</tr>
<tr class="odd">
<td>BlackEnergy</td>
<td><p>Power-grid attack → Ukraine blackout</p>
<p>Malware was involved in attacks on Ukraine's electricity grid in 2015.</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>OT attack flow (exam logic)</strong></p>
<p><strong>•</strong> IT breach → lateral move to OT → protocol abuse → process manipulation → PHYSICAL impact.</p>
<p><strong>•</strong> 'IT breach → OT damage.' Direct IT-to-OT communication is insecure.</p></td>
</tr>
</tbody>
</table>

# **8) Ports / protocols to remember**

| **Port** | **Protocol** | **Context**                                             |
|----------|--------------|---------------------------------------------------------|
| 1883     | MQTT         | IoT messaging (8883 = MQTT over TLS)                    |
| 5683     | CoAP         | IoT (over UDP)                                          |
| 502      | Modbus       | OT industrial control                                   |
| 20000    | DNP3         | OT power utilities                                      |
| 23       | Telnet       | IoT botnets brute-force this ('Telnet = IoT graveyard') |

# **9) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> An attacker uses Shodan to find thousands of internet-exposed IP cameras with default admin/admin logins.</p>
<p><strong>•</strong> A Mirai-style bot logs in via Telnet using those default credentials and installs malware.</p>
<p><strong>•</strong> The infected cameras form a botnet that floods a target with a massive DDoS.</p>
<p><strong>•</strong> Fix: change default credentials, disable Telnet, segment IoT onto its own VLAN, patch firmware.</p></td>
</tr>
</tbody>
</table>

# **10) Countermeasures**

## **IoT**

- Disable unused interfaces (JTAG/UART/debug); secure boot + hardware root of trust.

- Firmware: SIGNED + encrypted (encryption alone is not enough); secure OTA; remove hardcoded creds.

- Strong/cert-based auth, RBAC, least privilege; segment IoT (VLANs, never a flat network).

- Harden protocols: MQTT with auth + TLS, CoAP with DTLS, HTTP → HTTPS.

## **OT**

- **Zone & conduit model (IEC 62443):** group assets into zones, control traffic via conduits. 'Zones isolate, conduits control.'

- IT / DMZ / OT segmentation; MFA + role separation (operators ≠ administrators).

- Patch carefully in maintenance windows; passive monitoring + anomaly detection.

| **Standard**     | **Purpose**                  |
|------------------|------------------------------|
| IEC 62443        | OT security ('the OT bible') |
| NIST SP 800-82   | ICS security                 |
| OWASP IoT Top 10 | IoT risks                    |

# **11) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Most OT protocols (Modbus, DNP3) have NO native security.</p>
<p><strong>•</strong> Stuxnet = targeted PLCs / centrifuges (first cyber-physical weapon); Triton = safety systems; BlackEnergy = power grid.</p>
<p><strong>•</strong> JTAG &amp; UART = physical debug ports giving low-level/root access — disable in production.</p>
<p><strong>•</strong> MQTT without authentication lets anyone subscribe/publish ('open mic').</p>
<p><strong>•</strong> Mirai spreads via DEFAULT credentials over Telnet/SSH.</p>
<p><strong>•</strong> Shodan is a SEARCH ENGINE for exposed devices (recon), not an exploit tool.</p>
<p><strong>•</strong> Firmware ENCRYPTION alone is NOT enough — it must also be SIGNED.</p>
<p><strong>•</strong> OT can't be patched like IT; flat networks are NOT acceptable; direct IT-to-OT comms are insecure.</p></td>
</tr>
</tbody>
</table>

# **12) Mnemonics**

- **OT roles:** 'SCADA supervises, PLC controls, HMI shows.'

- **Purdue:** '0 = process, 5 = business.'

- **Botnet flow:** 'Scan, Infect, Control, Flood.'

- **OT kill path:** 'IT breach → OT damage.'

- **Defense:** 'Zones isolate, conduits control' · 'Signed + encrypted firmware.'

# **13) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> IoT = connected gadgets (cheap/old/exposed); OT = industrial control (old/critical). OT hacks cause PHYSICAL damage.</p>
<p><strong>•</strong> IoT protocols: MQTT (pub/sub, broker, port 1883), CoAP (UDP, 5683). OT: Modbus (502, no auth), DNP3 (power).</p>
<p><strong>•</strong> Device attacks: default creds, firmware tampering, JTAG/UART (debug ports), chip-off.</p>
<p><strong>•</strong> Mirai botnet: default creds + Telnet/SSH → DDoS. Flow: Scan → Infect → Control → Flood.</p>
<p><strong>•</strong> Tools: Shodan/Censys (find devices), Nmap (scan), RouterSploit/Metasploit (exploit).</p>
<p><strong>•</strong> OT components: PLC (control), RTU (remote), HMI (screen), SCADA (supervise).</p>
<p><strong>•</strong> Purdue levels 0-5: process → sensors → PLCs → SCADA/HMI → IT → enterprise.</p>
<p><strong>•</strong> Famous: Stuxnet (PLCs/centrifuges, 1st cyber-weapon), Triton (safety), BlackEnergy (grid).</p>
<p><strong>•</strong> OT attack flow: IT breach → lateral to OT → protocol abuse → process manipulation → physical impact.</p>
<p><strong>•</strong> Standards: IEC 62443 (OT), NIST SP 800-82 (ICS), OWASP IoT Top 10.</p>
<p><strong>•</strong> Defense: disable debug ports, signed+encrypted firmware, segment (VLAN/zones &amp; conduits), MQTT+TLS, no flat networks.</p></td>
</tr>
</tbody>
</table>

*End of Module 18 — IoT & OT Hacking. Next up: quiz this module, or move to Module 19 (Cloud Computing).*
