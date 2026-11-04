**CEH v13 — EXAM-FOCUSED SUMMARY**

**Module 20 — Cryptography**

*Beginner-friendly · Scenario-ready · What to memorize cold · FINAL MODULE*

Sources: Project documents (20.1–20.6 study sheets) + Google Drive (CEH V13 Master Study Guide). Mr-Infect notes style for mnemonics. General CEH knowledge where noted.

# **1) Definition**

**Cryptography = hiding information by turning readable plaintext into unreadable ciphertext using an algorithm (cipher) + a key**. Decryption reverses it. The word means 'hidden writing.'

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The locked-box picture</strong></p>
<p><strong>•</strong> Plaintext = your letter. The cipher = the lock. The key = what opens it.</p>
<p><strong>•</strong> Encryption locks the box; decryption unlocks it. Without the key, the box is useless to a thief.</p>
<p><strong>•</strong> Goals (CIA + N): Confidentiality, Integrity, Authentication, Non-repudiation. Encryption alone ≠ integrity or authentication.</p></td>
</tr>
</tbody>
</table>

# **2) Symmetric vs Asymmetric (the core split)**

|              | **Symmetric**         | **Asymmetric**                 |
|--------------|-----------------------|--------------------------------|
| Keys         | ONE shared secret key | TWO keys: public + private     |
| Speed        | Fast (bulk data)      | Slow (not for bulk)            |
| Main problem | Key distribution      | CPU-heavy                      |
| Used for     | Encrypting large data | Key exchange, signatures, auth |
| Also called  | Secret-key            | Public-key                     |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The golden rules</strong></p>
<p><strong>•</strong> 'Symmetric = fast, Asymmetric = trust.'</p>
<p><strong>•</strong> Public key ENCRYPTS, private key DECRYPTS. Private key SIGNS, public key VERIFIES.</p>
<p><strong>•</strong> EXAM TRAP: a public key CANNOT decrypt what it encrypted — only the matching private key can.</p></td>
</tr>
</tbody>
</table>

# **3) Symmetric algorithms**

| **Algorithm** | **Type (How data is encrypted)** | **Key size**               | **Block size** | **Structure (How the algorithm is built internally)** | **Mode of operation (How a block cipher processes data across blocks)** | **Security status**         |
|---------------|----------------------------------|----------------------------|----------------|-------------------------------------------------------|-------------------------------------------------------------------------|-----------------------------|
| DES           | Block                            | 56-bit                     | 64-bit         | Feistel                                               | ECB, CBC, CTR, etc.                                                     | ❌ Broken                   |
| 3DES          | Block                            | 112/168-bit nominal keying | 64-bit         | Feistel                                               | ECB, CBC, CTR, etc.                                                     | ⚠️ Deprecated; slow         |
| AES           | Block                            | 128/192/256-bit            | 128-bit        | SPN (not Feistel)                                     | ECB, CBC, CTR, GCM, etc.                                                | ✅ Secure                   |
| Blowfish      | Block                            | 32–448-bit                 | 64-bit         | Feistel                                               | Various block modes                                                     | ⚠️ Legacy; small block size |
| Twofish       | Block                            | 128/192/256-bit            | 128-bit        | Feistel-based                                         | Various block modes                                                     | ✅ Considered secure        |
| RC4           | Stream                           | Variable                   | No fixed block | Stream-cipher design                                  | Not applicable in the block-mode sense                                  | ❌ Insecure                 |
| ChaCha20      | Stream                           | 256-bit                    | No fixed block | Stream-cipher design                                  | Stream encryption; often paired with Poly1305                           | ✅ Secure                   |

| Mode                        | Simple meaning                                                                         | Exam takeaway                                  |
|-----------------------------|----------------------------------------------------------------------------------------|------------------------------------------------|
| ECB (Electronic Codebook)   | Encrypts each block independently.                                                     | ❌ **Reveals patterns; generally insecure.**   |
| CBC (Cipher Block Chaining) | Each plaintext block is combined with the previous ciphertext block before encryption. | Requires an IV.                                |
| CFB (Cipher Feedback)       | Uses previous ciphertext to generate encryption output.                                | Stream-like operation.                         |
| OFB (Output Feedback)       | Generates a keystream independently of plaintext.                                      | Stream-like operation.                         |
| CTR (Counter)               | Uses a counter to generate encryption output.                                          | Supports parallel processing.                  |
| GCM (Galois/Counter Mode)   | Encrypts data and checks whether it has been modified.                                 | ✅ Confidentiality + integrity/authentication. |

## **Block vs stream + modes**

- **Block cipher:** encrypts fixed blocks (AES, DES). Stream cipher: bit-by-bit (RC4, ChaCha20). 'Block = chunks, Stream = flow.'

- **Modes of operation:** ECB, CBC, CFB, OFB, CTR, GCM. EXAM TRAP: ECB is INSECURE (reveals patterns). 'Never use ECB.'

# **4) Asymmetric algorithms (very high yield)**

Know exactly what each one CAN and CANNOT do.

| **Algorithm**  | **Encrypt?** | **Sign?** | **Key exchange?** | **Key fact**                                      |
|----------------|--------------|-----------|-------------------|---------------------------------------------------|
| RSA            | Yes          | Yes       | Yes               | Based on factoring large numbers; the all-rounder |
| Diffie-Hellman | No           | No        | Yes               | Key EXCHANGE only; vulnerable to MITM             |
| DSA            | No           | Yes       | No                | SIGNATURES only                                   |
| ElGamal        | Yes          | Yes       | Yes               | Based on Diffie-Hellman; large ciphertext         |
| ECC            | Yes          | Yes       | Yes               | Small keys, fast (256-bit ECC ≈ 3072-bit RSA)     |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Exam traps (asymmetric)</strong></p>
<p><strong>•</strong> Diffie-Hellman does NOT encrypt — it only exchanges a shared secret (and is MITM-vulnerable without auth).</p>
<p><strong>•</strong> DSA = signatures only, cannot encrypt. RSA does all three.</p>
<p><strong>•</strong> ECC isn't 'stronger math' than RSA — it gives the same security with MUCH smaller keys (great for mobile/IoT).</p>
<p><strong>•</strong> Ephemeral DH (DHE/ECDHE) gives Perfect Forward Secrecy.</p></td>
</tr>
</tbody>
</table>

# **5) Hashing & message digests**

A hash is a one-way fingerprint of data — it provides INTEGRITY, not confidentiality. Hashing ≠ encryption (you can't reverse a hash).

**Hashing = Integrity. Encryption = Confidentiality.**

| **Hash**        | **Output** | **Status**             | **What to remember**                                             |
|-----------------|------------|------------------------|------------------------------------------------------------------|
| MD5             | 128-bit    | ❌ Broken (collisions) | Old algorithm; not safe for security-sensitive integrity checks. |
| SHA-1           | 160-bit    | ❌ Broken / deprecated | Also vulnerable to collisions.                                   |
| SHA-256 / SHA-2 | 256-bit    | ✅ Secure              | Widely used standard for general-purpose hashing.                |
| SHA-3           | Variable   | ✅ Secure              | Modern hashing family with a different design from SHA-2.        |

| **Concept**      | **Simple meaning**                                           | **Exam takeaway**                                                                  |
|------------------|--------------------------------------------------------------|------------------------------------------------------------------------------------|
| HMAC             | Hash + secret key                                            | Integrity + authentication; does not provide confidentiality.                      |
| Password hashing | Stores a password verifier instead of the original password. | Use slow algorithms such as bcrypt, scrypt, or PBKDF2.                             |
| Salt             | Random value added to password hashing.                      | Helps defeat rainbow-table attacks and makes identical passwords hash differently. |
| Slow hashing     | Makes each password guess more expensive.                    | Helps resist brute-force attacks.                                                  |

- **HMAC:** hash + secret KEY → gives integrity AND authentication (not confidentiality). 'HMAC = hash + key.'

- **Password hashing:** use SLOW, salted algorithms — bcrypt, scrypt, PBKDF2. 'Slow hashing = strong.'

- **Salt:** random value added before hashing → defeats rainbow-table (precomputed) attacks.

# **6) PKI & digital signatures**

**PKI (Public Key Infrastructure)** is the TRUST system that binds a public key to a real identity using certificates.

PKI (Public Key Infrastructure) is a complete system that allows computers and people to trust digital identities.

| **Component**                             | **Simple meaning**                                                              | **CEH exam takeaway**                           |
|-------------------------------------------|---------------------------------------------------------------------------------|-------------------------------------------------|
| PKI                                       | The complete system for managing certificates and trust.                        | Trust system                                    |
| CA (Certificate Authority)                | Trusted authority that issues and digitally signs certificates.                 | Signs certificates                              |
| Digital certificate                       | Connects an identity, such as a website, to a public key.                       | Identity + public key                           |
| X.509                                     | Standard format for digital certificates.                                       | Certificate format                              |
| Trust chain                               | Links a website's certificate to a trusted root CA.                             | Root CA → Intermediate CA → Website certificate |
| CRL (Certificate Revocation List)         | A published list of certificates that have been revoked.                        | Revocation list                                 |
| OCSP (Online Certificate Status Protocol) | Checks a certificate's revocation status online.                                | Online status check                             |
| Self-signed certificate                   | A certificate signed by its own creator, not automatically trusted by browsers. | Not automatically trusted                       |

## **Digital signature process**

- Hash the message → encrypt the hash with the SENDER'S PRIVATE key → send message + signature.

- Receiver decrypts the signature with the sender's PUBLIC key → re-hashes the message → compares. Match = authentic + untampered.

- **Memory hook:** 'Sign = private, verify = public.' CRL = periodic list; OCSP = real-time. Self-signed certs are untrusted.

| Step       | What happens?                                                                                    |
|------------|--------------------------------------------------------------------------------------------------|
| 1\. Sign   | Sender hashes the message and signs the hash using their private key.                            |
| 2\. Send   | Sender sends the message + digital signature.                                                    |
| 3\. Verify | Receiver uses the sender's public key to verify the signature and checks the message's hash.     |
| 4\. Result | Successful verification confirms the signature is valid and the message matches what was signed. |

# 

# **7) Hybrid cryptosystem (how TLS/HTTPS works)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>The best of both worlds</strong></p>
<p><strong>•</strong> Step 1 — use ASYMMETRIC (RSA/ECC) to securely exchange a random SESSION KEY.</p>
<p><strong>•</strong> Step 2 — use SYMMETRIC (AES) with that session key to encrypt all the actual data (fast).</p>
<p><strong>•</strong> This is why HTTPS is both secure (<strong>trust from asymmetric</strong>) and fast (<strong>bulk from symmetric</strong>).</p></td>
</tr>
</tbody>
</table>

# **8) Cryptography attacks (high yield)**

| **Attack**               | **Simple meaning — what the attacker does**                                                              | **Defense — how to prevent it**                                                                |
|--------------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Brute force              | Tries every possible key until the correct one is found.                                                 | Use long keys, e.g. AES-256.                                                                   |
| Birthday attack          | Exploits the probability of finding two inputs with the same hash.                                       | Use strong, sufficiently long hashes such as SHA-256 or SHA-3.                                 |
| Collision attack         | Finds two different inputs that produce the same hash.                                                   | Avoid MD5 and SHA-1; use SHA-256 or SHA-3.                                                     |
| MITM (Man-in-the-Middle) | Intercepts communication between two parties, potentially replacing their exchanged keys.                | Authenticate the key exchange; use certificates and signed key exchanges.                      |
| Side-channel attack      | Uses information leaked by a device, such as timing, power consumption, or electromagnetic signals.      | Constant-time code, secure hardware design, and shielding where appropriate.                   |
| Padding oracle           | Exploits differences in error responses to reveal information about encrypted data, often with CBC mode. | Use authenticated encryption such as AES-GCM and avoid leaking distinguishable padding errors. |
| Downgrade attack         | Forces systems to use an older, weaker protocol or cryptographic option.                                 | Disable obsolete protocols and weak cipher suites.                                             |
| Replay attack            | Captures valid data or an authentication message and sends it again later.                               | Use nonces, timestamps, sequence numbers, and replay detection.                                |
| Rainbow-table attack     | Uses precomputed tables to match stolen password hashes to likely passwords.                             | Use unique salts and slow password-hashing algorithms such as bcrypt, scrypt, or PBKDF2.       |

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Misconfig = the real-world killer</strong></p>
<p><strong>•</strong> Most crypto fails in IMPLEMENTATION: weak algorithms, short keys, poor randomness, reused IVs, ECB mode. 'Crypto fails in implementation.'</p></td>
</tr>
</tbody>
</table>

# 

# 

# 

# **9) Tools (CEH recognition)**

| **Tool**        | **Purpose (simple meaning)**                                                          | **Free or Paid?**                                           | **Memory hook**         |
|-----------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------|-------------------------|
| Hashcat         | Cracks password hashes by testing possible passwords.                                 | Free (open source)                                          | Fast hash cracking      |
| John the Ripper | Cracks password hashes using different techniques.                                    | Free (community version is open source; Jumbo is also free) | Password cracker        |
| Cain & Abel     | Legacy Windows tool for recovering passwords and analyzing credentials.               | Free (historically freeware; discontinued/obsolete)         | Windows credentials     |
| OpenSSL         | Performs cryptographic operations, manages keys, and works with certificates.         | Free (open source)                                          | Crypto Swiss Army knife |
| CrypTool        | Helps users learn and experiment with encryption, hashing, and cryptographic attacks. | Free (open source)                                          | Learn cryptography      |

## **OpenSSL commands (recognition)**

openssl enc \# encrypt / decrypt

openssl dgst \# generate a hash

openssl genrsa \# generate an RSA key

openssl req \# create a CSR (cert signing request)

# **10) Related concepts**

| Concept                                      | Simple meaning — what should I understand?                                                                                                                                        | Memory hook                         |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------|
| Key Escrow / GAK (Government Access to Keys) | A third party stores a copy of encryption keys so it can provide access under defined conditions, such as lawful authorization.                                                   | Someone else holds a key copy.      |
| Perfect Forward Secrecy (PFS)                | If a long-term private key is stolen later, attackers still cannot use it alone to decrypt previously recorded sessions protected by properly implemented ephemeral key exchange. | Past sessions stay safe.            |
| Cryptanalysis                                | The study of finding weaknesses in encryption or breaking cryptographic protection to recover information without simply being given the key.                                     | Analyze how to break crypto.        |
| Nonce (Number used once)                     | A value used in a cryptographic operation to help ensure freshness or uniqueness and, in protocols, prevent replay when correctly checked.                                        | Fresh value, not reused improperly. |
| Cryptosystem                                 | The complete system used to protect information, including algorithms, keys, protocols, and their implementation.                                                                 | The whole crypto system.            |

# **11) Exam traps (read twice)**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Traps that catch people</strong></p>
<p><strong>•</strong> Public key encrypts &amp; verifies; private key decrypts &amp; signs. Public key CANNOT decrypt its own ciphertext.</p>
<p><strong>•</strong> Diffie-Hellman = key exchange ONLY (no encryption, no signing); MITM-vulnerable without auth.</p>
<p><strong>•</strong> DSA = signatures only. RSA = encrypt + sign + exchange.</p>
<p><strong>•</strong> ECC = same security as RSA with much SMALLER keys (not different strength).</p>
<p><strong>•</strong> DES broken, 3DES slow, AES standard, RC4 broken (ChaCha20 replaces it).</p>
<p><strong>•</strong> ECB mode is insecure (reveals patterns). AES block is ALWAYS 128-bit.</p>
<p><strong>•</strong> MD5 &amp; SHA-1 are broken; SHA-2/SHA-3 secure. Hashing ≠ encryption; HMAC = hash + key.</p>
<p><strong>•</strong> Salt defeats rainbow tables; padding oracle targets CBC; birthday attack targets hash collisions.</p>
<p><strong>•</strong> CRL = periodic revocation list; OCSP = real-time status.</p></td>
</tr>
</tbody>
</table>

# **12) Mnemonics**

- **Goals:** 'CIA + N' (Confidentiality, Integrity, Authentication, Non-repudiation).

- **Keys:** 'Public encrypts, private decrypts; private signs, public verifies.'

- **Speed vs trust:** 'Symmetric = fast, Asymmetric = trust.'

- **Algorithm status:** 'DES Dead, 3DES Tired, AES Alive' · 'RC4 Rapidly Cracked.'

- **Signatures:** 'Sign = private, verify = public.'

- **Hashes:** 'Hash = fingerprint' · 'Salt defeats rainbow tables.'

# **13) What to memorize COLD**

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Fast recall before the exam</strong></p>
<p><strong>•</strong> Goals = CIA + N. Encryption alone ≠ integrity/auth.</p>
<p><strong>•</strong> Symmetric = 1 key, fast, bulk (AES). Asymmetric = 2 keys, slow, trust (RSA).</p>
<p><strong>•</strong> Public encrypts/verifies; private decrypts/signs.</p>
<p><strong>•</strong> Symmetric: DES (broken), 3DES (slow), AES (standard, 128-bit block, not Feistel), RC4 (broken), ChaCha20 (modern).</p>
<p><strong>•</strong> ECB insecure; prefer CBC/CTR/GCM.</p>
<p><strong>•</strong> Asymmetric: RSA (all three), DH (exchange only, MITM), DSA (sign only), ECC (small keys), ElGamal.</p>
<p><strong>•</strong> Ephemeral DH (DHE/ECDHE) = Perfect Forward Secrecy.</p>
<p><strong>•</strong> Hashing = integrity, one-way. MD5/SHA-1 broken; SHA-2/3 secure. HMAC = hash + key. Salt stops rainbow tables.</p>
<p><strong>•</strong> Password hashing: bcrypt, scrypt, PBKDF2 (slow + salted).</p>
<p><strong>•</strong> PKI: CA signs certs; chain Root→Intermediate→End; CRL (periodic) vs OCSP (real-time); self-signed = untrusted.</p>
<p><strong>•</strong> Digital signature: hash → encrypt with private key → verify with public key.</p>
<p><strong>•</strong> Hybrid (TLS): asymmetric exchanges a session key, symmetric (AES) encrypts the data.</p>
<p><strong>•</strong> Attacks: brute force, birthday (collisions), MITM (DH), side-channel (physical), padding oracle (CBC), downgrade, replay, rainbow table.</p>
<p><strong>•</strong> Tools: Hashcat, John the Ripper, OpenSSL (enc/dgst/genrsa/req).</p></td>
</tr>
</tbody>
</table>

*End of Module 20 — Cryptography. This completes the full v3 set (Modules 01–20). Next: quiz this module, a full mock exam, or a last-minute revision sheet.*
