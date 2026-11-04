**Module 04 — Enumeration**

If Scanning (Module 03) was walking the perimeter checking which doors and windows are unlocked, **Enumeration is walking through the unlocked door and opening every drawer, cabinet, and closet** to see exactly what's inside — usernames, shared folders, printers, employee lists. This is **Phase 3** of the 5 Hacking Phases, and it's part of the **24%-weighted "Network & Perimeter Hacking"** domain alongside Scanning.

**1. What is Enumeration? 🏠**

Per your Master Study Guide and confirmed by supplementary CEH v13 notes: Enumeration is the process of **extracting usernames, machine names, network resources, shares, and services** from a system or network by creating **active connections** and sending **directed queries**.

**Key distinguishing trait (confirmed across sources):** Enumeration is **always active** — there's no "passive enumeration." You must connect to extract this level of detail.

**What attackers specifically collect during enumeration:**

- Network resources & shares

- Routing tables

- Audit and service settings

- SNMP and FQDN (fully qualified domain name) details

- Machine names

- Users and groups

- Applications and banners

**SNMP = Simple Network Management Protocol**

Used to **monitor and manage network devices** such as routers, switches, servers, and printers.

**CEH key point:**

**SNMP can be used for Enumeration** to gather information such as:

- Device names

- Interfaces

- IP addresses

- Network configuration

- System information

**Port:** UDP **161** (SNMP queries)  
**Port:** UDP **162** (SNMP traps)

**2. Scanning vs. Enumeration — Locking In the Distinction**

|                           | **Scanning (Phase 2)**            | **Enumeration (Phase 3)**                                                                 |
|---------------------------|-----------------------------------|-------------------------------------------------------------------------------------------|
| **Scope (Walker's rule)** | Extranet-focused                  | Intranet-focused                                                                          |
| **Interaction level**     | Active, but broad/surface         | Active, deep/detailed queries                                                             |
| **Goal**                  | "What's alive and open?"          | "What exactly can I extract from what's alive?"                                           |
| **House analogy**         | Checking which doors are unlocked | Walking inside and reading the mail on the counter, checking the family photo on the wall |

**3. The 7 Enumeration Techniques (by protocol) — Core of This Module**

| **Technique**           | **Protocol / Port** | **What You Extract**                                              | **Key Tools**                              | **Easy Functionality / Memory**                                                   |
|-------------------------|---------------------|-------------------------------------------------------------------|--------------------------------------------|-----------------------------------------------------------------------------------|
| **NetBIOS Enumeration** | **Ports 137–139**   | **Shares, usernames, computer names, domain info**                | **nbtstat, nbtscan, net view, enum4linux** | **🖥️ “Who/what is on the Windows network?” → computers, users, shares**           |
| **SNMP Enumeration**    | **UDP 161/162**     | **Network devices, routing tables, ARP tables, traffic stats**    | **snmp-check, SNMPwalk**                   | **📡 “Tell me about the network device.” → routers, interfaces, routes, traffic** |
| **LDAP Enumeration**    | **Port 389**        | **Active Directory info — users, groups, organization structure** | **ldapsearch, ldp.exe**                    | **👥 “Tell me about the directory.” → users, groups, AD structure**               |
| **NTP Enumeration**     | **UDP 123**         | **System time, connected hosts, network info**                    | **ntptrace, ntpq**                         | **🕐 “What time is it and who is connected?”**                                    |
| **NFS Enumeration**     | **Port 2049**       | **Shared directories/files on Unix/Linux**                        | **showmount, rpcinfo**                     | **📁 “What Linux folders are being shared?”**                                     |
| **SMTP Enumeration**    | **Port 25**         | **Valid usernames/email addresses using VRFY, EXPN, RCPT TO**     | **telnet, smtp-user-enum**                 | **✉️ “Which email/users are valid?”**                                             |
| **DNS Enumeration**     | **Port 53**         | **Subdomains, zone-transfer data, mail servers**                  | **dnsenum, Fierce, dig**                   | **🌐 “Tell me about the domain.” → subdomains, mail servers, DNS records**        |

| **Technique** | **Command / Example**                                           | **What you're asking**                                 |
|---------------|-----------------------------------------------------------------|--------------------------------------------------------|
| **NetBIOS**   | **nbtstat -A 192.168.1.10**                                     | **🖥️ “Tell me the Windows/NetBIOS info.”**             |
| **SNMP**      | **snmpwalk -v2c -c public 192.168.1.10**                        | **📡 “Give me information from this network device.”** |
| **LDAP**      | **ldapsearch -x -H ldap://192.168.1.10 -b "dc=example,dc=com"** | **👥 “Show me directory/users information.”**          |
| **NTP**       | **ntpq -p 192.168.1.10**                                        | **🕐 “Show me NTP/peer information.”**                 |
| **NFS**       | **showmount -e 192.168.1.10**                                   | **📁 “What directories are being shared?”**            |
| **SMTP**      | **telnet 192.168.1.10 25 then VRFY john**                       | **✉️ “Does user John exist?”**                         |
| **DNS**       | **dig example.com**                                             | **🌐 “Give me DNS information for this domain.”**      |

**🧠 Super-short memory**

- **NetBIOS → 🖥️ Windows computers/users/shares**

- **SNMP → 📡 Network devices**

- **LDAP → 👥 Users & groups / Active Directory**

- **NTP → 🕐 Time**

- **NFS → 📁 Linux shared folders**

- **SMTP → ✉️ Email users**

- **DNS → 🌐 Domain/subdomains**

**The key idea is:**

**Enumeration = asking a specific protocol: “Give me information about yourself.”**

**Bonus techniques mentioned in official courseware objectives:** IPsec, VoIP, RPC, Linux/Unix, and SMB enumeration are also covered as "other enumeration techniques" — know they exist even if less heavily tested.

**4. NetBIOS Enumeration — Deep Dive (heavily tested)**

NetBIOS lets applications communicate over a LAN. A classic attack path:

1.  Attacker finds a **NULL session** or **IPC\$ share** (a Windows inter-process communication share)

2.  Uses this to probe further, potentially connecting to an **administrative share**

3.  Brute-forces admin credentials to gain full file-system access

**Key commands:**

**nbtstat -A \<target-ip\> → NetBIOS name table**

**net view \\target-ip → List shared resources**

**net use \\target-ip\ipc\$ "" /user:"" → NULL session connection**

**enum4linux \<target-ip\> → All-in-one NetBIOS/SMB enum tool**

**5. SNMP Enumeration — Why It's Dangerous**

SNMP (Simple Network Management Protocol) manages routers, switches, and hubs — but it authenticates using "**community strings**" that are often left at their **default values**:

- public — read-only access

- private — read-write access (far more dangerous if exposed)

**Exam trap:** If a scenario mentions a device still using the default "public" or "private" community string, that's an SNMP misconfiguration vulnerability — very testable.

**6. Enumeration Countermeasures (defender's side)**

- Disable unnecessary services (NetBIOS, SNMP) if not needed

- Change default SNMP community strings immediately

- Configure SMTP servers to **not reveal internal host/IP info** in error messages, and ignore mail to unknown recipients

- Restrict null session access on Windows systems

- Implement SPF, DKIM, and DMARC to reduce email-based enumeration/spoofing

- Regularly patch NFS server software against known vulnerabilities

**🎯 WHAT TO MEMORIZE COLD FOR THE EXAM (Module 04)**

1.  **Enumeration = always ACTIVE** (no passive form exists) — key distinguishing trait

2.  **Enumeration = intranet-focused**, Scanning = extranet-focused (Walker's rule)

3.  **All 7 enumeration techniques and their ports**: NetBIOS (137-139), SNMP (161/162), LDAP (389), NTP (123), NFS (2049), SMTP (25), DNS (53)

4.  **SNMP default community strings**: "public" (read-only) vs "private" (read-write)

5.  **NULL session / IPC\$ attack path** on Windows — a classic exam scenario

6.  **Tool-to-protocol mapping**: enum4linux (NetBIOS/SMB), snmp-check (SNMP), LDAPsearch (LDAP), dnsenum/Fierce (DNS)

7.  **What enumeration extracts**: usernames, shares, machine names, routing tables, groups — vs. scanning which only finds live hosts/ports

**Exam behavior tip specific to this module:** Expect direct port-to-protocol matching questions and scenario questions describing a NULL session or default SNMP string being exploited — you'll need to name the enumeration type and the specific risk involved.

SNMP + default public/private = misconfiguration → public reads, private writes.

IPC\$ is a special Windows share used for **communication between computers/processes**.

A **NULL session** means trying to connect **without providing a username/password**:

Instead of manually running many enumeration checks; enum4linux is basically an **all-in-one Windows/SMB enumeration tool**.

NetBIOS/SMB enumeration = discover the Windows machine → users/names → shares → test anonymous access → look for weaknesses.

**SPF/DKIM/DMARC → Protect your domain from fake/spoofed emails.**

**NFS patching → Keep the file-sharing server protected against known vulnerabilities.**
