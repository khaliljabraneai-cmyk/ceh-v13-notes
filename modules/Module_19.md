**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 19 — Cloud Computing**

*Beginner-friendly · Scenario-ready · What to memorize cold*

Sources: Project documents (19.1–19.4 study sheets) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

Cloud computing = on-demand delivery of IT capabilities (servers, storage, apps) over the internet, billed per use (metered). Hacking the cloud mostly means exploiting MISCONFIGURATION, weak IAM, and insecure APIs — not breaking the provider's hardware.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>The renting picture</strong></p>
<p><strong>•</strong> Instead of owning servers, you RENT computing from AWS/Azure/Google and pay only for what you use.</p>
<p><strong>•</strong> The provider secures the building (hardware, data center); YOU secure what you put inside (data, access, config).</p>
<p><strong>•</strong> Core truth: the cloud is NOT automatically secure — <mark>most breaches are the customer's misconfiguration</mark>.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **2) Key characteristics (NIST)**

- On-demand self-service · Broad network access · Resource pooling (multi-tenant) · Rapid elasticity · Measured service (pay-per-use).

- **Memory hook:** 'On-demand, pooled, elastic, measured.'

| Characteristic             | Simple meaning                                                            | Example                                               | Characteristic             |
|----------------------------|---------------------------------------------------------------------------|-------------------------------------------------------|----------------------------|
| 1\. On-demand self-service | Get resources whenever you need them, without contacting a person.        | Create a virtual machine (VM) yourself.               | 1\. On-demand self-service |
| 2\. Broad network access   | Access cloud services over a network using different devices.             | Access Google Drive from a PC or smartphone.          | 2\. Broad network access   |
| 3\. Resource pooling       | A provider shares resources between multiple customers (multi-tenancy).   | Multiple companies use the same cloud infrastructure. | 3\. Resource pooling       |
| 4\. Rapid elasticity       | Increase or decrease resources quickly when needed.                       | Add servers when website traffic increases.           | 4\. Rapid elasticity       |
| 5\. Measured service       | Resource usage is monitored and measured; you often pay for what you use. | Pay for the hours your cloud VM runs.                 | 5\. Measured service       |

# **3) Service models (very high yield)**

The more 'as-a-service' you buy, the LESS you control (and the less you must secure).

| **Model** | **What it provides**               | **You control**      | **Examples**                  |
|-----------|------------------------------------|----------------------|-------------------------------|
| IaaS      | Virtual machines, storage, network | OS, apps, data       | AWS EC2, Azure VM, GCE        |
| PaaS      | App development platform           | Your app code only   | App Engine, Azure App Service |
| SaaS      | Ready-to-use applications          | Just your data/usage | Gmail, Salesforce, M365       |

- **Other models:** IDaaS (identity/SSO/MFA — Okta, Azure AD), SECaaS (security), CaaS (containers — EKS/GKE), FaaS (serverless — Lambda), XaaS (anything).

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Memory hook</strong></p>
<p><strong>•</strong> 'IaaS = rent hardware, PaaS = build apps, SaaS = use software.'</p>
<p><strong>•</strong> 'More service = less control.'</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **4) Deployment models**

| **Model**   | **Meaning**                                                            |
|-------------|------------------------------------------------------------------------|
| Public      | Owned by a third-party provider, shared over the internet (AWS, Azure) |
| Private     | Single organization, high security, high cost                          |
| Community   | Shared by several organizations with common needs (e.g., regulatory)   |
| Hybrid      | Mix of public + private                                                |
| Multi-cloud | Several providers (avoids vendor lock-in)                              |

- **Memory hook:** 'Hybrid = mix, Multi = many.'

# **5) Shared Responsibility Model (ABSOLUTELY tested)**

Who secures what depends on the service model. The customer always owns Data + IAM.

| **Layer**      | **On-Prem** | **IaaS** | **PaaS** | **SaaS** |
|----------------|-------------|----------|----------|----------|
| Applications   | User        | User     | User     | Provider |
| Data           | User        | User     | User     | User\*   |
| OS             | User        | User     | Provider | Provider |
| Virtualization | User        | Provider | Provider | Provider |
| Hardware       | User        | Provider | Provider | Provider |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Exam traps (shared responsibility)</strong></p>
<p><strong>•</strong> 'Provider secures the cloud; customer secures what's IN the cloud.'</p>
<p><strong>•</strong> The CUSTOMER is responsible for breaches caused by misconfiguration.</p>
<p><strong>•</strong> Customer ALWAYS owns Data + IAM across all models. Security is NEVER fully the provider's job.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# 

# 

# **6) Cloud threats & attacks (high yield)**

| **Threat / attack**             | **Key point**                                                                      |
|---------------------------------|------------------------------------------------------------------------------------|
| Misconfiguration                | \#1 cause of cloud breaches (public S3 buckets, open storage, default creds)       |
| Insecure interfaces & APIs      | APIs are the PRIMARY cloud attack surface                                          |
| Account/service hijacking       | Stolen creds = 'keys to the kingdom' (full control)                                |
| Data breach / data loss         | Weak IAM, misconfig, ransomware, accidental deletion                               |
| Metadata service attack         | SSRF to the metadata endpoint → steal instance credentials                         |
| VM Escape                       | Break out of a virtual machine to reach the hypervisor or host. Rare but critical. |
| Container Escape                | Escape a privileged or misconfigured container that shares the host kernel.        |
| Cloud Malware Injection         | Introduce a fake, malicious cloud instance that appears legitimate.                |
| Side-Channel Attack             | Use cache-timing leaks to obtain information from co-resident VMs.                 |
| Shared Technology Vulnerability | A hypervisor flaw may enable attacks across different cloud tenants.               |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>The headline rule</strong></p>
<p><strong>•</strong> Most cloud breaches = MISCONFIGURATION + weak IAM + insecure APIs. 'Misconfig = data leak.'</p>
<p><strong>•</strong> Metadata attack chain: SSRF → metadata endpoint → credentials.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **7) Attack flow & tools**

- **Cloud attack flow:** Recon (OSINT) → identify misconfiguration → exploit IAM/API → escalate privileges → persist via keys/roles.

- **Privilege escalation is POLICY-based:** role chaining, policy abuse, bad trust relationships — NOT kernel exploits.

| **Tool**                  | **Purpose**                                   |
|---------------------------|-----------------------------------------------|
| Pacu                      | AWS exploitation framework                    |
| Prowler                   | AWS security auditing                         |
| ScoutSuite                | Multi-cloud security auditing                 |
| CloudSploit / Nuclei      | Misconfiguration scanning                     |
| GCPBucketBrute            | Google Cloud Storage bucket enumeration       |
| MicroBurst / Stormspotter | Azure pentest / attack-path mapping           |
| Shodan / Censys / Amass   | Cloud reconnaissance (finding exposed assets) |

# 

# 

# 

# **8) Concepts to remember**

| **Concept**        | **Very simple meaning**                                                                         | **What you should remember for CEH V13**                       |
|--------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| Security Group     | A firewall protecting a cloud VM.                                                               | STATEFUL = return traffic is automatically allowed.            |
| Network ACL (NACL) | A firewall filtering traffic for a subnet.                                                      | STATELESS = inbound and outbound rules are checked separately. |
| Metadata Service   | A service inside a cloud VM that provides information about the VM and may provide credentials. | Attackers may use SSRF to steal cloud credentials.             |
| CloudTrail         | A record of actions performed in AWS.                                                           | Shows who did what and when. Helps investigate attacks.        |
| CSPM               | A tool that checks cloud security settings for mistakes.                                        | Finds misconfigurations, such as publicly exposed storage.     |

- **Exam trap:** Security Groups are STATEFUL; NACLs are STATELESS.

 **SSRF (Server-Side Request Forgery):** An attacker tricks a server into sending requests to a destination chosen by the attacker.

# **9) Practical example (scenario style)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Scenario walk-through</strong></p>
<p><strong>•</strong> An app on an EC2 instance has an SSRF flaw. The attacker makes it request http://169.254.169.254/ (the metadata service).</p>
<p><strong>•</strong> The metadata endpoint returns the instance's IAM role credentials (no auth needed from the VM).</p>
<p><strong>•</strong> The attacker uses those creds via the API, finds an over-permissive IAM policy, and escalates through role chaining.</p>
<p><strong>•</strong> They persist by creating new access keys, then delete CloudTrail logs to cover tracks.</p>
<p><strong>•</strong> Fix: block/limit metadata (IMDSv2), least-privilege IAM, patch SSRF, protect log integrity.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **10) Countermeasures**

- **IAM (first line of defense):** least privilege, roles instead of long-lived keys, MFA, key rotation, no root-account use, regular audits.

- **Network:** Security Groups (stateful) + NACLs (stateless), private subnets, bastion hosts.

- **Data:** encrypt at rest + in transit, KMS/HSM key management, tokenization. (Encryption alone does NOT prevent breaches.)

- **Storage:** disable public access, bucket policies, access logging, versioning.

- **Compute/containers:** patch VMs, trusted + scanned images, runtime monitoring, least-privilege containers.

- **Monitoring:** enable logging by default (CloudTrail/CloudWatch), centralize + protect log integrity, anomaly detection (GuardDuty, Defender for Cloud).

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Countermeasure flow</strong></p>
<p><strong>•</strong> 'IAM → Network → Data → Monitor → Respond.' Standards: ISO 27001, GDPR, HIPAA, PCI DSS.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **11) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Cloud is NOT secure by default; the provider does NOT handle all security.</p>
<p><strong>•</strong> Customer owns Data + IAM in every model and is liable for misconfiguration breaches.</p>
<p><strong>•</strong> Most cloud breaches = misconfiguration (public buckets) + weak IAM + insecure APIs.</p>
<p><strong>•</strong> APIs are the PRIMARY attack surface; cloud attacks are mostly config/identity, not network exploits.</p>
<p><strong>•</strong> Cloud privilege escalation is POLICY-based (role chaining), not kernel-based.</p>
<p><strong>•</strong> Metadata attack = SSRF → metadata → steal credentials.</p>
<p><strong>•</strong> Security Groups = STATEFUL, NACLs = STATELESS.</p>
<p><strong>•</strong> Encryption protects data but does NOT prevent breaches; logging is NOT optional (attackers delete logs).</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **12) Mnemonics**

- **Service models:** 'IaaS rent hardware, PaaS build apps, SaaS use software' · 'More service = less control.'

- **Responsibility:** 'Provider secures OF the cloud, customer secures IN the cloud.'

- **Breaches:** 'Misconfig = data leak' · 'APIs are the cloud.'

- **Metadata:** 'SSRF → metadata → creds.'

- **Defense:** 'IAM → Network → Data → Monitor.'

# **13) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Cloud = on-demand, pooled, elastic, measured IT over the internet. Not secure by default.</p>
<p><strong>•</strong> Service models: IaaS (OS+apps+data), PaaS (app code), SaaS (data only). More service = less control.</p>
<p><strong>•</strong> Deployment: public, private, community, hybrid (mix), multi-cloud (many).</p>
<p><strong>•</strong> Shared responsibility: provider secures the cloud; customer secures IN the cloud + always owns Data &amp; IAM.</p>
<p><strong>•</strong> Top threats: misconfiguration (public S3), insecure APIs (primary surface), account hijacking, data breach.</p>
<p><strong>•</strong> Attacks: metadata (SSRF→creds), VM escape, container escape, malware injection, side-channel.</p>
<p><strong>•</strong> Attack flow: Recon → misconfig → exploit IAM/API → escalate (policy/role chaining) → persist via keys/roles.</p>
<p><strong>•</strong> Security Groups = stateful; NACLs = stateless.</p>
<p><strong>•</strong> Tools: Pacu, Prowler, ScoutSuite, CloudSploit (attack/audit); GuardDuty/Defender (defend).</p>
<p><strong>•</strong> Defense: least-privilege IAM + MFA + roles, disable public storage, encrypt + KMS, enable + protect logs.</p>
<p><strong>•</strong> Encryption doesn't stop breaches; logging isn't optional.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

*End of Module 19 — Cloud Computing. Next up: quiz this module, or move to Module 20 (Cryptography) — the final module.*
