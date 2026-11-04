**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 17 — Hacking Mobile Platforms**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Project documents (17.1–17.5 study sheets) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

**Hacking mobile platforms** means attacking phones/tablets (Android, iOS), their apps, the networks they use, and the backend cloud. **Mobiles** are **high-value targets** because they are **always connected** and **hold personal** + **corporate data** (banking, OTPs).

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The pocket-computer picture</strong></p>
<p><strong>•</strong> A phone is a full computer that you carry everywhere, always online, holding your logins, OTPs and messages.</p>
<p><strong>•</strong> Attackers reach it <strong><mark>on three fronts</mark></strong>: the DEVICE (OS/apps), the NETWORK (Wi-Fi/MITM), and the CLOUD (backend servers).</p>
<p><strong>•</strong> Core sentence: always-on + personal data = prime target.</p></td>
</tr>
</tbody>
</table>

# **2) The 3 attack points & mobile attack surface**

| **Point**               | **Target**                 | **Example attacks**                                 |
|-------------------------|----------------------------|-----------------------------------------------------|
| 1\. Device              | OS, apps, hardware         | Rooting/jailbreak, malicious apps, insecure storage |
| 2\. Network             | Wi-Fi, cellular, Bluetooth | Wi-Fi sniffing, MITM, SSL stripping, rogue AP       |
| 3\. Data center / Cloud | Web servers, databases     | SQLi, XSS, misconfig, brute force                   |

**Exam takeaway:** A mobile application can be attacked at three levels: the device, the network, and the backend infrastructure. Securing only the phone is not enough.

## **OWASP Mobile Top 10 (2024) — memorize the list**

| **ID** | **Risk**                              | **Simple explanation to remember**                                                          |
|--------|---------------------------------------|---------------------------------------------------------------------------------------------|
| M1     | Improper Credential Usage             | Mishandling passwords, API keys, or login credentials.                                      |
| M2     | Inadequate Supply Chain Security      | Using compromised or vulnerable third-party libraries or components.                        |
| M3     | Insecure Authentication/Authorization | Weak login checks or allowing users to access data they shouldn't.                          |
| M4     | Insufficient Input/Output Validation  | Failing to check input or output, potentially allowing malicious data or injection attacks. |
| M5     | Insecure Communication                | Sending data over insecure connections, allowing interception or manipulation.              |
| M6     | Inadequate Privacy Controls           | Collecting, using, or exposing more personal data than necessary.                           |
| M7     | Insufficient Binary Protections       | Making the app easier to reverse-engineer, modify, or repackage.                            |
| M8     | Security Misconfiguration             | Incorrect security settings, such as debug mode enabled in production.                      |
| M9     | Insecure Data Storage                 | Storing sensitive data insecurely on the phone, such as passwords in plain text.            |
| M10    | Insufficient Cryptography             | Using weak encryption, outdated algorithms, or incorrect cryptographic implementation.      |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook</strong></p>
<p><strong>•</strong> 'Vectors → Android → iOS → MDM → Defense' (the 5 objectives).</p>
<p><strong>•</strong> After compromise: Spy → Steal → Spread → Impersonate (surveillance, data theft, botnet, impersonation).</p></td>
</tr>
</tbody>
</table>

# **3) Android vs iOS (very high yield)**

| **Feature**           | **Android**                       | **iOS**                   |
|-----------------------|-----------------------------------|---------------------------|
| Source model          | Open-source (Linux-based, Google) | Closed-source (Apple)     |
| Root access term      | Rooting                           | Jailbreaking              |
| App vetting           | Weak (third-party stores allowed) | Strong (App Store review) |
| Custom ROMs           | Yes                               | No                        |
| Enterprise-cert abuse | Less                              | More                      |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Memory hook</strong></p>
<p><strong>•</strong> 'Android = open risk, iOS = controlled risk.'</p>
<p><strong>•</strong> Root (Android) = Jailbreak (iOS): both gain superuser and BREAK the sandbox + MDM controls.</p>
<p><mark><strong>Custom ROM</strong>: An alternative Android system installed by the user.</mark></p>
<p><mark><strong>Enterprise-cert abuse:</strong> Misusing a company's app-distribution certificate to install malicious apps outside the official app store.</mark></p></td>
</tr>
</tbody>
</table>

# **4) Android threats & attacks**

- **Architecture:** Linux Kernel → HAL → Native libraries → Android Runtime (ART) → App framework → Apps.

<img src="media/image1.png" style="width:3.52324in;height:2.83015in" />

## **Key Android attacks**

| Attack                    | Simple explanation                                                                                 |
|---------------------------|----------------------------------------------------------------------------------------------------|
| Repackaging               | Attacker takes a legitimate app, adds malicious code, and redistributes it as a fake app.          |
| Drive-by download         | Visiting a malicious website triggers a malware download, sometimes without the user's awareness.  |
| SMS-based attacks         | Fake SMS messages trick users (smishing), cause premium SMS charges, or help steal OTP codes.      |
| Man-in-the-Mobile (MitMo) | Mobile malware steals banking information or intercepts SMS verification codes to bypass security. |
| Clickjacking              | A hidden or misleading screen tricks the user into tapping something unintended.                   |
| Android botnets           | Infected phones are remotely controlled to send spam, launch DDoS attacks, or steal data.          |

## **Rooting & permission abuse**

- **Rooting = superuser access:** disables sandboxing, bypasses permissions, breaks MDM. 'Root = no rules.'

- **Rooting** does not automatically disable all sandboxing or permissions, but it can weaken security and undermine MDM controls.

- **Dangerous permissions:** READ_SMS (OTP theft), SEND_SMS (fraud), RECORD_AUDIO/CAMERA (spy), LOCATION (tracking). 'SMS = money, mic = spy.'

| **Android permission identifier** | **What it allows**                       | **Security risk — how it can be abused**                                                                                    |
|-----------------------------------|------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| READ_SMS                          | Read SMS messages                        | OTP theft: Malware reads verification codes sent by SMS and may use them to access the victim's accounts.                   |
| SEND_SMS                          | Send SMS messages                        | SMS fraud: A malicious app sends messages to premium-rate numbers, potentially causing unexpected charges.                  |
| RECORD_AUDIO                      | Record sound                             | Eavesdropping: Malware records private conversations or listens to surrounding sounds when it has the necessary access.     |
| CAMERA                            | Access the camera                        | Surveillance: A malicious app may capture photos or videos without the user's informed consent, subject to OS restrictions. |
| LOCATION                          | Access the device's location, if granted | Tracking: Malware collects location data to discover where the victim lives, works, or travels.                             |

## **ADB (Android Debug Bridge) — know the commands**

## **ADB = a tool that lets a computer communicate with an Android device for debugging and administration.**

## **Memory hook: devices = list, shell = command line, pull = phone → PC, push = PC → phone, install = APK installation.**

## 

adb devices \# list connected devices

adb shell \# open a shell on the device

adb pull / push \# copy files from / to the device

adb install app.apk \# install an APK

# **5) iOS threats & attacks**

- **iOS security model:** code signing (only signed apps run) + sandboxing + secure boot chain + App Store vetting + Data Protection API. 'Sign → Sandbox → Secure Boot.'

<img src="media/image2.png" style="width:3.96639in;height:2.95517in" />

## 

## **Jailbreaking (iOS root access)**

| **Jailbreak type** | **Meaning**                                        |
|--------------------|----------------------------------------------------|
| Tethered           | Needs a computer at every boot                     |
| Semi-tethered      | Boots, but needs a computer for jailbreak features |
| Untethered         | Persistent — survives reboot without a computer    |

- **Impact:** disables code signing, bypasses sandbox, breaks MDM. 'Jailbreak = root access.'

**CEH V13 — Jailbreaking (iOS)**

- Meaning: Modify an iPhone to bypass some Apple restrictions and gain greater control over iOS.

- **Tethered**: Needs a computer to boot the jailbroken device.

- **Semi-tethered**: iPhone boots normally, but jailbreak features stop until restored using a computer.

- **Untethered**: Jailbreak remains active after reboot without a computer.

- Security impact: May weaken code signing, app sandboxing, and MDM protections.

## **Key iOS attacks**

| **Attack**                    | **How it works**                                                             |
|-------------------------------|------------------------------------------------------------------------------|
| Malicious apps                | Via third-party stores / jailbroken devices                                  |
| Enterprise certificate abuse  | Misuse Apple enterprise certs to install unsigned apps ('bypass gatekeeper') |
| Configuration profile attacks | Malicious profile installs rogue VPN/proxy/cert → traffic interception       |
| iOS spyware                   | Call recording, SMS/GPS tracking, app-data theft                             |
| Network-based                 | Rogue Wi-Fi, MITM, SSL stripping, fake certificates                          |

# **6) Mobile Device Management (MDM)**

MDM = centralized control of mobile devices: push policies, manage apps, and perform remote actions.

| **Remote action**            | **Use**                                                             |
|------------------------------|---------------------------------------------------------------------|
| Remote wipe / Selective wipe | Erase all data, or just corporate data (lost device)                |
| Device lock / Password reset | Secure a lost or compromised device                                 |
| Containerization             | Separate corporate vs personal data (BYOD-friendly, selective wipe) |

- **MDM vs EMM vs UEM:** MDM (device) ⊂ EMM (device+apps+content) ⊂ UEM (unified endpoint). Tools: Intune, Workspace ONE, MaaS360, MobileIron, Meraki.

MDM is a general technology concept, not the name of one specific software. ✅

Think of it like this:

- MDM = the technology/category used to manage mobile devices.

- Microsoft Intune = a commercial product that provides MDM features.

- VMware Workspace ONE = another commercial product that provides MDM features.

**Simple example**

A company has 500 employee phones. Instead of configuring every phone manually, the IT administrator uses MDM software to manage them centrally.

| **Term**         | **Full name**                  | **Example / meaning**                                         |
|------------------|--------------------------------|---------------------------------------------------------------|
| MDM              | Mobile Device Management       | Technology to manage mobile devices                           |
| EMM              | Enterprise Mobility Management | Manages devices, apps, and corporate content                  |
| UEM              | Unified Endpoint Management    | Manages multiple endpoint types centrally                     |
| Microsoft Intune | Commercial product name        | Software providing MDM and broader device-management features |
| Workspace ONE    | Commercial product name        | Enterprise device and workspace management solution           |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam trap — MDM is NOT invincible</strong></p>
<p><strong>•</strong> MDM cannot stop zero-day exploits.</p>
<p><strong>•</strong> Rooted/jailbroken devices bypass MDM controls.</p>
<p><strong>•</strong> It depends on user compliance and is limited against social engineering.</p></td>
</tr>
</tbody>
</table>

# **7) Tools (know tool → purpose)**

| **Tool**   | **Platform** | **Purpose**                              |
|------------|--------------|------------------------------------------|
| Drozer     | Android      | Security assessment / attack surface     |
| APKTool    | Android      | Reverse-engineer / repackage APKs        |
| JADX       | Android      | Decompile DEX to Java                    |
| ADB        | Android      | Device control via command line          |
| Frida      | Both         | Runtime instrumentation (hook functions) |
| Objection  | Both         | Runtime manipulation (built on Frida)    |
| MobSF      | Both         | Automated static+dynamic app analysis    |
| Cydia      | iOS          | Package manager on jailbroken devices    |
| Burp Suite | Both         | Intercept/MITM app traffic               |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Analysis types (MAST)</strong></p>
<p><strong>•</strong> SAST = static (reads code/binary) · DAST = dynamic (runtime behavior) · IAST = interactive (both). 'Static sees code, Dynamic sees behavior.'</p></td>
</tr>
</tbody>
</table>

# Below are 3 types of mobile application security testing:

# **SAST (Static Application Security Testing**): Examine the code to find a hardcoded password.

# **DAST (Dynamic Application Security Testing)**: Run the app and test whether its login accepts an attack.

# **IAST (Interactive Application Security Testing):** Run the app with an analysis agent to identify a vulnerability as the vulnerable code executes.

# **8) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> An attacker takes a popular free game, decompiles it with APKTool, inserts spyware, re-signs it, and posts it on a third-party store (repackaging).</p>
<p><strong>•</strong> A victim side-loads it; the app requests READ_SMS and RECORD_AUDIO permissions.</p>
<p><strong>•</strong> It now steals banking OTPs (READ_SMS) and records the mic — classic permission abuse.</p>
<p><strong>•</strong> If the phone is rooted, sandboxing is gone and the malware persists and evades MDM.</p>
<p><strong>•</strong> Fix: install only from official stores, review permissions, avoid rooted devices, enforce MDM compliance checks.</p></td>
</tr>
</tbody>
</table>

# **9) Countermeasures & guidelines**

- **Device:** strong screen lock + biometrics, encrypt storage, disable USB debugging, enable remote wipe, patch OS.

- **Apps:** install only from trusted stores, review permissions, avoid rooted/jailbroken devices, remove unused apps.

- **Network:** avoid public Wi-Fi, use VPN, disable auto-connect, verify SSL certificates (block user-installed CAs).

- **Data:** encrypt at rest and in transit, no plaintext storage, secure key management, secure backups.

- **Enterprise:** enforce MDM + containerization + compliance/posture checks.

## **Attack → defense mapping (exam favorite)**

| **Attack**     | **Defense**              |
|----------------|--------------------------|
| Malware        | App vetting + MDM        |
| Smishing       | User awareness           |
| MITM           | VPN + TLS                |
| Root/Jailbreak | Device compliance checks |
| Data leakage   | Encryption               |
| Rogue Wi-Fi    | Disable auto-connect     |

# **10) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Rooting = Android; Jailbreaking = iOS. Both give root and break the sandbox + MDM.</p>
<p><strong>•</strong> Android = open-source (weak vetting); iOS = closed-source (strong vetting) but still attacked.</p>
<p><strong>•</strong> Untethered jailbreak = persistent (survives reboot); tethered needs a PC each boot.</p>
<p><strong>•</strong> Man-in-the-Mobile (MitMo) = mobile malware beating banking apps via overlay + SMS interception.</p>
<p><strong>•</strong> Enterprise certificate abuse lets UNSIGNED apps run on iOS (bypasses the gatekeeper).</p>
<p><strong>•</strong> MDM can't stop zero-days and is bypassed on rooted/jailbroken devices.</p>
<p><strong>•</strong> ADB is the Android control channel (adb shell, pull, push, install).</p></td>
</tr>
</tbody>
</table>

# **11) Mnemonics**

- **Objectives:** 'Vectors, Android, iOS, MDM, Defense.'

- **Attack points:** 'Device → Network → Cloud.'

- **After compromise:** 'Spy, Steal, Spread, Impersonate.'

- **Android vs iOS:** 'Open risk vs controlled risk' · 'Root = Jailbreak.'

- **Permissions:** 'SMS = money, mic = spy.'

- **MDM scope:** 'MDM ⊂ EMM ⊂ UEM.'

# **12) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Mobile = high-value (always on + personal data). 3 attack points: Device, Network, Cloud.</p>
<p><strong>•</strong> OWASP Mobile Top 10 2024: M1 Improper Credential Usage ... M9 Insecure Data Storage, M10 Insufficient Cryptography.</p>
<p><strong>•</strong> Android = open-source, rooting, weak vetting, custom ROMs. iOS = closed, jailbreaking, strong vetting.</p>
<p><strong>•</strong> Root/Jailbreak: disables sandbox, bypasses permissions, breaks MDM.</p>
<p><strong>•</strong> Android attacks: repackaging, drive-by, smishing, MitMo, clickjacking, botnets.</p>
<p><strong>•</strong> Dangerous perms: READ_SMS (OTP), SEND_SMS (fraud), RECORD_AUDIO/CAMERA (spy), LOCATION.</p>
<p><strong>•</strong> iOS security: code signing + sandbox + secure boot. Jailbreak types: tethered, semi-tethered, untethered (persistent).</p>
<p><strong>•</strong> iOS attacks: malicious apps, enterprise cert abuse, config profiles, spyware, network MITM.</p>
<p><strong>•</strong> ADB commands: devices, shell, pull, push, install.</p>
<p><strong>•</strong> Tools: Drozer, APKTool, JADX, ADB (Android); Cydia (iOS); Frida, Objection, MobSF, Burp (both).</p>
<p><strong>•</strong> MDM: remote wipe, selective wipe, lock, containerization. MDM ⊂ EMM ⊂ UEM. Not invincible vs zero-day/root.</p>
<p><strong>•</strong> Defense: official stores, review permissions, VPN on public Wi-Fi, encrypt data, enforce MDM.</p></td>
</tr>
</tbody>
</table>

*End of Module 17 — Hacking Mobile Platforms. Next up: quiz this module, or move to Module 18 (IoT & OT Hacking).*
