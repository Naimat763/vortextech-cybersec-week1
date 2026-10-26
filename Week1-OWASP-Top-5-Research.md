# Vortex Tech Academy — Week 1 Cybersecurity Research
## Research the OWASP Top 10:2025 Vulnerabilities

**Prepared by:** Naimat Ullah  
**Track:** Cyber Security Internship Track  
**Training organization:** Vortex Tech Academy  
**Research date:** 26 October 2026  
**Reference edition:** OWASP Top 10:2025

## Introduction

The OWASP Top 10 is an awareness document that highlights important risks to web applications. This report reviews five categories from the current OWASP Top 10:2025 list. Each section explains the risk in plain language, describes a high-level attack scenario, summarizes a documented real-world case, and lists practical prevention measures for developers and organizations.

**Selected categories:**
1. A01:2025 — Broken Access Control
2. A02:2025 — Security Misconfiguration
3. A03:2025 — Software Supply Chain Failures
4. A04:2025 — Cryptographic Failures
5. A07:2025 — Authentication Failures

The examples below are researched case studies, not claims that I personally tested the affected companies. All practical testing should remain within owned systems or explicitly authorized training labs.

---

## 1. A01:2025 — Broken Access Control

### What it is

Broken Access Control happens when an application does not correctly enforce what a user is allowed to view or do. A logged-in user may be able to access another person's records, use an admin function, or perform an action outside their role.

### How an attacker could exploit it

An attacker may change an object identifier in a URL or API request, or request an admin-only endpoint directly. If the server checks only whether the user is logged in and does not verify ownership or role permissions for that specific resource, private information or restricted functions may become accessible.

### Real-world case and impact — First American Financial (2019)

In 2019, a flaw in First American Financial's document-sharing application exposed more than 800 million document images dating back to 2003. The documents included sensitive personal and financial information. Public reporting described how changing document numbers in URLs could expose other records without proper authentication. The U.S. Securities and Exchange Commission later described the vulnerability and brought charges related to the company's cybersecurity disclosure controls. The case demonstrates the serious privacy impact of failing to protect each document with server-side access checks.

### Practical prevention / mitigation

- Enforce authorization in trusted server-side code for every request to a protected resource.
- Check both the user's role and ownership/permission for the specific record.
- Deny access by default; do not rely on hidden buttons or unpredictable IDs as the security control.
- Test direct URL/API access using accounts with different permissions.
- Log repeated authorization failures and review suspicious patterns.

**Sources:**
- OWASP A01:2025 — Broken Access Control: https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/
- U.S. SEC, First American Financial cybersecurity disclosure controls (15 June 2021): https://www.sec.gov/newsroom/press-releases/2021-102
- New York DFS filing / findings: https://www.dfs.ny.gov/system/files/documents/2021/07/ea20200721_first_american_noh_revised.pdf

---

## 2. A02:2025 — Security Misconfiguration

### What it is

Security Misconfiguration occurs when an application, server, cloud service, or database is configured unsafely. Examples include public cloud storage that should be private, unnecessary services left enabled, default credentials, weak security headers, or overly detailed error messages.

### How an attacker could exploit it

An attacker can look for publicly reachable storage, exposed admin pages, default accounts, directory listings, or services that should not be available from the internet. If permissions are too broad, the attacker may be able to read sensitive information without first exploiting a complicated software bug.

### Real-world case and impact — Verizon/NICE Systems cloud exposure (2017)

In 2017, a publicly accessible Amazon S3 repository operated by Verizon vendor NICE Systems exposed customer call records and account details. UpGuard estimated that information relating to as many as 14 million U.S. customers was exposed; Verizon said approximately 6 million unique customers were affected. The exposed information included names, addresses, account details, and some account PINs. Verizon said it found no evidence that customer information had been lost or stolen, so confirmed misuse should not be assumed. Nevertheless, the public exposure created a substantial privacy and account-fraud risk and illustrated why cloud permissions and third-party storage configurations need review.

### Practical prevention / mitigation

- Block public access to private storage by default and explicitly approve any public content.
- Apply least-privilege permissions to cloud buckets, identities, APIs, and service accounts.
- Use repeatable hardened configurations for development, testing, and production.
- Scan cloud configuration continuously and review changes before deployment.
- Remove default accounts, unused services, sample applications, and unnecessary ports.
- Include third-party vendors and their storage environments in security reviews.

**Sources:**
- OWASP A02:2025 — Security Misconfiguration: https://top10.owasp.org/2025/A02_2025-Security_Misconfiguration/
- UpGuard, Cloud Leak: How a Verizon Partner Exposed Millions of Customer Accounts (12 July 2017): https://www.upguard.com/breaches/verizon-cloud-leak
- CIO Dive summary of Verizon/NICE Systems exposure: https://www.ciodive.com/news/third-party-vendor-blamed-for-verizon-data-leak/447080/

---

## 3. A03:2025 — Software Supply Chain Failures

### What it is

Software Supply Chain Failures occur when software, a dependency, a development/build system, or an update/distribution process is vulnerable or compromised. An organization can be affected even if its own application code was not directly written by the attacker.

### How an attacker could exploit it

An attacker may compromise a developer account, build server, software dependency, package repository, or vendor update process. If a malicious or tampered update is trusted and installed, the attacker's code can run inside otherwise legitimate software and potentially reach many downstream organizations.

### Real-world case and impact — SolarWinds Orion (2020)

In 2020, attackers compromised the software supply chain associated with SolarWinds Orion and inserted the SUNBURST backdoor into affected software updates. CISA reported that the activity affected U.S. government agencies, critical infrastructure entities, and private-sector organizations. The incident showed how trust in a legitimate vendor update can be abused to reach multiple organizations at scale.

### Practical prevention / mitigation

- Maintain an inventory and Software Bill of Materials (SBOM) for direct and transitive dependencies.
- Scan components and build pipelines for known vulnerabilities and unusual changes.
- Protect source-code repositories and CI/CD systems with MFA, least privilege, branch protection, and isolated build permissions.
- Verify build provenance and artifact integrity; sign releases and protect signing keys.
- Separate build, test, and release duties, and keep tamper-evident logs.
- Monitor vendor advisories and have a tested process to pause, revoke, or replace a suspect update.

**Sources:**
- OWASP A03:2025 — Software Supply Chain Failures: https://top10.owasp.org/2025/A03_2025-Software_Supply_Chain_Failures/
- CISA, Active Exploitation of SolarWinds Software (13 December 2020; revised 14 December 2020): https://www.cisa.gov/news-events/alerts/2020/12/13/active-exploitation-solarwinds-software
- CISA advisory details: https://content.govdelivery.com/accounts/USDHSCISA/bulletins/2b1b32d

---

## 4. A04:2025 — Cryptographic Failures

### What it is

Cryptographic Failures happen when sensitive data is not protected with appropriate cryptography, or when encryption, hashing, key management, randomness, or certificate validation is implemented incorrectly. This can make supposedly protected information readable or recoverable.

### How an attacker could exploit it

An attacker who obtains a database or intercepted data may take advantage of weak or reversible password storage, weak algorithms, reused keys, hard-coded secrets, or missing transport encryption. If password hints or other clues are also exposed, recovering passwords may become easier.

### Real-world case and impact — Adobe (2013)

Adobe announced in October 2013 that attackers accessed customer IDs and encrypted passwords, and that information relating to 2.9 million customers—including encrypted payment-card details—had been removed from its systems. Have I Been Pwned later catalogued approximately 153 million Adobe accounts, including encrypted passwords and plaintext password hints, and noted that the password cryptography was poorly implemented and many passwords were quickly recovered. The company announcement and breach-database figures describe different parts/measurements of the incident; they should not be treated as identical counts. The incident illustrates why sensitive credentials need modern, purpose-built protection rather than weak or reversible storage.

### Practical prevention / mitigation

- Store passwords with a modern password-hashing scheme such as Argon2id, scrypt, or appropriately configured bcrypt/PBKDF2, using unique salts and suitable work factors.
- Never store plaintext passwords or recoverable encrypted passwords when password hashing is appropriate.
- Use established cryptographic libraries and current, well-reviewed algorithms rather than custom cryptography.
- Encrypt sensitive data in transit and at rest, and validate TLS certificates correctly.
- Store keys in an appropriate secret manager/HSM, restrict access, and rotate keys when exposure is suspected.
- Keep only the sensitive data that the business actually needs.

**Sources:**
- OWASP A04:2025 — Cryptographic Failures: https://top10.owasp.org/2025/A04_2025-Cryptographic_Failures/
- Adobe, Important Customer Security Announcement (3 October 2013): https://blog.adobe.com/en/publish/2013/10/03/important-customer-security-announcement
- Have I Been Pwned, Adobe Data Breach: https://haveibeenpwned.com/breach/adobe

---

## 5. A07:2025 — Authentication Failures

### What it is

Authentication Failures occur when a system does not reliably prove that a user is who they claim to be. Weak passwords, reused/stolen credentials, missing multi-factor authentication (MFA), poor session handling, and missing protections against automated login attempts can contribute to this risk.

### How an attacker could exploit it

An attacker may use credentials stolen by phishing, infostealer malware, or an earlier data breach to sign in as a legitimate user. Without MFA, rate limits, suspicious-login detection, and strong session controls, stolen credentials may be enough to access private data.

### Real-world case and impact — Snowflake customer-account attacks (2024)

In 2024, Mandiant investigated a campaign in which attackers used previously stolen customer credentials to access Snowflake customer instances, steal data, and attempt extortion. Mandiant stated that the incidents it investigated traced back to compromised customer credentials and that the affected accounts did not have MFA enabled at the time. Mandiant did not find evidence that unauthorized access in those cases resulted from a breach of Snowflake's own enterprise environment. This distinction matters: the campaign demonstrated the risk of weak account protection and credential reuse at customer organizations.

### Practical prevention / mitigation

- Require MFA, especially for administrators, cloud consoles, remote access, and sensitive data platforms.
- Check new and changed passwords against known-breached password lists and encourage password managers.
- Block or slow brute-force and credential-stuffing attempts with rate limits and risk-based controls.
- Detect logins from unusual locations/devices and investigate abnormal data access or exports.
- Revoke sessions and reset credentials promptly when compromise is suspected.
- Reduce exposure to infostealer malware with endpoint protection, patching, and user awareness.

**Sources:**
- OWASP A07:2025 — Authentication Failures: https://top10.owasp.org/2025/A07_2025-Authentication_Failures/
- Google Cloud / Mandiant, UNC5537 Targets Snowflake Customer Instances for Data Theft and Extortion: https://cloud.google.com/blog/topics/threat-intelligence/unc5537-snowflake-data-theft-extortion

---

## Personal Reflection (4 sentences)

The vulnerability that surprised me most was Broken Access Control because one missing server-side permission check can expose a very large number of private records. The First American case helped me understand that a web application must verify access to every individual document instead of trusting a URL or a logged-in session alone. I also learned that security depends on more than code: cloud configuration, software updates, cryptography, and account protection all matter. This Week 1 research strengthened my interest in application security and encouraged me to keep practicing in authorized labs and documenting what I learn.

## Conclusion

The five case studies show that web security failures can result from missing permission checks, unsafe configuration, compromised software updates, weak cryptographic protection, and inadequate authentication. Developers and security teams should combine secure design and implementation with configuration review, supply-chain controls, strong cryptography, multi-factor authentication, monitoring, and regular testing.

## Main Reference: Current OWASP Top 10

- OWASP Top 10:2025 official list: https://owasp.org/Top10/2025/
- OWASP Top 10:2025 project: https://top10.owasp.org/2025/

---

**Academic integrity note:** This report is written in original, beginner-friendly wording and summarizes the linked sources. Real-world case descriptions are attributed to the named sources. The report is for cybersecurity education and defensive awareness.
