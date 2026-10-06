# Mukesh VAPT — Security Research Portfolio

> **Vulnerability Assessment & Penetration Testing | Web & API Security | Cloud & Infrastructure | Mobile Security | Bug Bounty Research**

A practical security-research portfolio documenting **25 independently identified security findings**, vulnerability validation, responsible disclosure, and technical analysis across web applications, APIs, cloud infrastructure, authentication systems, mobile ecosystems, and backend services.

The focus is not the number of reports. It is the ability to **find a security boundary, validate it, demonstrate realistic impact, understand the root cause, and communicate the result clearly**.

---

## ⚡ Security Research at a Glance

| Signal | Portfolio evidence |
|---|---:|
| **Documented findings** | **25** |
| **High-severity research** | **2** |
| **Medium-severity research** | **1** |
| **Public security recognition** | **RepAutomate 2026 Hall of Fame** |
| **Research channels** | HackerOne, Bugcrowd, vendor security programs |
| **Primary focus** | Web, API, authentication, authorization, cloud/infrastructure |
| **Advanced research themes** | SSRF, DoS, TOCTOU, sandbox isolation, telemetry integrity, wallet revocation |

> Severity and program outcomes are reported as actually assessed or communicated by the relevant program where available. The portfolio does not convert duplicate, informative, or out-of-scope reports into artificial severity claims.

---

## 🔥 Featured Research

These are the findings I recommend reviewing first when evaluating technical depth.

### 1. High — Unbounded Request Body DoS in cardano-submit-api
**Finding #16**

A request-body handling path materialized attacker-controlled HTTP input into memory without an effective maximum size before conversion to a strict ByteString.

- **Severity:** High
- **CVSS:** 7.5
- **Class:** Resource Exhaustion / DoS
- **Research signal:** Source-level root-cause analysis + controlled validation
- **Impact:** Potential memory exhaustion and service unavailability

→ [Read Finding #16](bug-bounty/writeups/16-cardano-submit-api-unbounded-request-body-dos.md)

### 2. High — TOCTOU Race Causing ouroboros-consensus Node Crash
**Finding #17**

I identified a time-of-check/time-of-use race involving chain state and copyToImmutableDB, where stale state could reach a fatal error path.

- **Severity:** High
- **CVSS:** 7.5 provisional
- **CWE:** CWE-367
- **Research signal:** Source-code reasoning + deterministic two-phase reproduction model
- **Impact:** Node crash / availability loss

→ [Read Finding #17](bug-bounty/writeups/17-ouroboros-consensus-toctou-copytoimmutabledb-node-crash.md)

### 3. Medium — Authenticated Blind SSRF
**Finding #15**

A domain-verification workflow could cause the backend to initiate HTTPS connections to attacker-controlled destinations, including link-local address space.

- **Severity:** Medium
- **CWE:** CWE-918
- **Research signal:** Server-side behavior validated through controlled infrastructure and timing
- **Impact:** Potential internal-network reconnaissance

→ [Read Finding #15](bug-bounty/writeups/15-authenticated-blind-ssrf-domain-asset-verification.md)

### 4. Unauthenticated Remote Crash — Dovecot Stats Service
**Finding #20**

Malformed HTTP chunked input sent to the OpenMetrics endpoint triggered a worker crash. Testing also showed that in-memory telemetry counters were reset after the worker respawned.

- **Access:** Unauthenticated
- **Class:** Remote crash / availability + telemetry impact
- **Research signal:** Protocol-level testing + process behavior validation

→ [Read Finding #20](bug-bounty/writeups/20-dovecot-stats-unauthenticated-malformed-chunked-crash.md)

### 5. Wallet Lifecycle / Revocation Failure
**Finding #25**

Deleting an MDVM wallet did not cascade revocation to associated WPB, RWSCA, and PNS capabilities.

The particularly important validation was that **re-registering with the same authentication key produced a new token and new wi_id, yet the new wallet could still operate the surviving RWSCA account and generate a fresh valid ECDSA signature**.

- **Research theme:** Authentication lifecycle / authorization state
- **Impact:** Deleted/decommissioned wallet capabilities remain usable
- **Program outcome:** Duplicate of #4046235

→ [Read Finding #25](bug-bounty/writeups/25-mdvm-deletion-fails-wpb-rwsca-pns-revocation.md)

### 6. Production Authentication Origin Exposure
**Finding #24**

A production authentication service behind an in-scope hostname was directly reachable through public AWS origin infrastructure outside the intended Cloudflare edge path.

- **Research theme:** Cloud / infrastructure security boundary
- **Validation:** Origin forced-resolution and application-level response comparison
- **Impact:** Cloudflare-only controls may not apply to direct-origin traffic
- **Program outcome:** Duplicate of #3620121

→ [Read Finding #24](bug-bounty/writeups/24-banco-plata-cloudflare-origin-auth-exposure.md)

### 7. Forged Telemetry Injection via Public OTel Ingestion
**Finding #22**

Unauthenticated OpenTelemetry logs, metrics, and traces ingestion accepted attacker-controlled telemetry.

- **Research theme:** Observability / telemetry integrity
- **Validation:** HTTP 201 acceptance of unauthenticated telemetry
- **Impact:** Potential forged service telemetry, misleading alerts, and reduced investigation reliability
- **Program outcome:** Duplicate of #3592648

→ [Read Finding #22](bug-bounty/writeups/22-crypto-com-unauthenticated-otel-telemetry-injection.md)

---

## 🏆 Public Security Recognition

### RepAutomate — 2026 Security Hall of Fame

I was publicly recognized by **RepAutomate** in its 2026 Security Hall of Fame for a responsible security contribution involving **Clickjacking**.

- **Researcher:** Mukesh Adla
- **Recognition:** 2026 Security Hall of Fame
- **Contribution:** Clickjacking vulnerability
- **Date:** May 2026
- **Disclosure:** Responsible disclosure

🔗 [View the RepAutomate Security Hall of Fame](https://repautomate.co.za/security/hall-of-fame/)

📄 [Read the technical write-up](bug-bounty/writeups/01-clickjacking-repautomate.md)

---

## 🧠 What My Research Demonstrates

### Security Boundary Analysis
- Authentication and authorization boundary testing
- Account lifecycle and revocation analysis
- Cloud edge vs. origin trust boundaries
- Entitlement enforcement
- Sandbox and privilege-boundary analysis

### Server-Side Security
- SSRF
- Resource-exhaustion DoS
- Request parsing weaknesses
- HTTP protocol edge cases
- Worker/process crash analysis
- Race-condition and TOCTOU reasoning

### API Security
- Authentication enforcement
- Authorization and entitlement controls
- API input validation
- Excessive authentication attempts
- Unauthenticated data/operational endpoints
- API error and configuration disclosure

### Application & Client Security
- Clickjacking
- CORS
- CSP weaknesses
- Hardcoded third-party credentials
- Public mobile application artifacts
- JavaScript and endpoint analysis

### Security Research Methodology
- Reconnaissance and attack-surface mapping
- Hypothesis-driven testing
- Manual validation of automated findings
- Root-cause analysis
- Impact validation
- Evidence preservation
- Responsible disclosure
- Careful scope and limitation statements

---

## 📊 Research Portfolio

| # | Research / Contribution | Outcome | Publication |
|---|---|---|---|
| 01 | Clickjacking — RepAutomate | **Publicly acknowledged / Hall of Fame** | [Finding #1](bug-bounty/writeups/01-clickjacking-repautomate.md) |
| 02 | Clickjacking & Scope Validation | Out of Scope | [Finding #2](bug-bounty/writeups/02-clickjacking-scope-validation.md) |
| 03 | Verbose GraphQL Error Disclosure | Informational (P5) | [Finding #3](bug-bounty/writeups/03-graphql-error-disclosure-lesson.md) |
| 04 | OAuth Client Identifier / API Authorization | Duplicate / N/A | [Finding #4](bug-bounty/writeups/04-oauth-client-identifier-authentication-lesson.md) |
| 05 | CORS Application Configuration Disclosure | Low | [Finding #5](bug-bounty/writeups/05-cors-application-configuration-disclosure.md) |
| 06 | MCP Token Creation / Plan Entitlement | Entitlement inconsistency | [Finding #6](bug-bounty/writeups/06-mcp-token-entitlement.md) |
| 07 | Insufficient Authentication Rate Limiting | CWE-307 | [Finding #7](bug-bounty/writeups/07-login-rate-limiting-cwe-307.md) |
| 08 | Public Password Policy Configuration | Low | [Finding #8](bug-bounty/writeups/08-nucleus-password-policy-disclosure.md) |
| 09 | Public Management Server Status | Low | [Finding #9](bug-bounty/writeups/09-nucleus-server-status-disclosure.md) |
| 10 | Weak Content Security Policy | Informational | [Finding #10](bug-bounty/writeups/10-nucleus-csp-misconfiguration.md) |
| 11 | Hardcoded Third-Party API Key | Duplicate | [Finding #11](bug-bounty/writeups/11-alaan-hardcoded-api-key.md) |
| 12 | Unauthenticated Report Endpoint / Operational Data Exposure | Responsible disclosure | [Finding #12](bug-bounty/writeups/12-unauthenticated-report-operational-data.md) |
| 13 | Weak Password Policy / Low-Entropy Password Acceptance | Low / Informational | [Finding #13](bug-bounty/writeups/13-weak-password-policy-low-entropy.md) |
| 14 | Password Change Allows Current Password Reuse | Low / Informational | [Finding #14](bug-bounty/writeups/14-password-change-allows-current-password-reuse.md) |
| 15 | Authenticated Blind SSRF — Domain Asset Verification | **Medium / CWE-918** | [Finding #15](bug-bounty/writeups/15-authenticated-blind-ssrf-domain-asset-verification.md) |
| 16 | Unbounded Request Body — cardano-submit-api DoS | **High / CVSS 7.5** | [Finding #16](bug-bounty/writeups/16-cardano-submit-api-unbounded-request-body-dos.md) |
| 17 | TOCTOU Race — ouroboros-consensus Node Crash | **High / CWE-367 / CVSS 7.5** | [Finding #17](bug-bounty/writeups/17-ouroboros-consensus-toctou-copytoimmutabledb-node-crash.md) |
| 18 | Authenticated FILTER SIEVE SCRIPT Worker Crash — Dovecot | Informative | [Finding #18](bug-bounty/writeups/18-dovecot-filter-sieve-script-worker-crash.md) |
| 19 | doveadm HTTP API uint32 Validation Crash — Dovecot | Informative | [Finding #19](bug-bounty/writeups/19-dovecot-doveadm-uint32-validation-worker-crash.md) |
| 20 | Unauthenticated Stats Service Crash — Dovecot | Remote crash / telemetry loss | [Finding #20](bug-bounty/writeups/20-dovecot-stats-unauthenticated-malformed-chunked-crash.md) |
| 21 | Vercel Sandbox Privilege Boundary Escalation | Not Applicable / Out of Scope | [Finding #21](bug-bounty/writeups/21-vercel-sandbox-proc1-mem-auth-bypass-root.md) |
| 22 | Unauthenticated OpenTelemetry Telemetry Injection — Crypto.com | Duplicate | [Finding #22](bug-bounty/writeups/22-crypto-com-unauthenticated-otel-telemetry-injection.md) |
| 23 | Public Firebase Storage Bucket — Unico IDtech | Duplicate / Informative | [Finding #23](bug-bounty/writeups/23-unico-public-firebase-storage-enumeration-download.md) |
| 24 | Production Authentication Origin Exposure — Banco Plata | Duplicate / Informative | [Finding #24](bug-bounty/writeups/24-banco-plata-cloudflare-origin-auth-exposure.md) |
| 25 | MDVM Account Deletion Lifecycle Issue — d-you EUDI Wallet Ecosystem | Duplicate | [Finding #25](bug-bounty/writeups/25-mdvm-deletion-fails-wpb-rwsca-pns-revocation.md) |

> **Important:** Duplicate, informative, and out-of-scope outcomes are intentionally preserved. A portfolio should distinguish **what was found** from **how a program ultimately classified it**.

---

## 🔬 High-Signal Research Themes

| Theme | Representative findings |
|---|---|
| **Authentication & Authorization** | #4, #6, #7, #13, #14, #25 |
| **SSRF / Server-Side Request Behavior** | #15 |
| **DoS / Availability** | #16, #17, #18, #19, #20 |
| **Cloud / Infrastructure Boundaries** | #21, #23, #24 |
| **API Security & Input Validation** | #3, #6, #7, #12, #16, #19, #22 |
| **Telemetry / Observability Integrity** | #20, #22 |
| **Security Configuration / Information Disclosure** | #3, #5, #8, #9, #10, #23 |
| **Client / Web Security** | #1, #2, #5, #10, #11 |
| **Wallet / Cryptographic Lifecycle** | #25 |

---

## 🎯 Interview / VAPT Capability Map

| If you want to assess… | Strongest portfolio evidence |
|---|---|
| **API Security** | #4, #6, #7, #12, #15, #16, #19, #22 |
| **Authentication & Authorization** | #4, #6, #7, #14, #25 |
| **SSRF** | #15 — authenticated blind SSRF with server-side request validation |
| **DoS / Availability** | #16, #17, #18, #19, #20 |
| **Race Conditions / Concurrency** | #17 — TOCTOU analysis and deterministic reproduction |
| **Cloud / Infrastructure Security** | #21, #23, #24 |
| **Web Security** | #1, #2, #5, #10, #11 |
| **Mobile Security** | #23 — Android APK and Firebase Storage exposure analysis |
| **Source / Root-Cause Analysis** | #16, #17, #18, #19, #20, #25 |
| **Business / Security Logic** | #6, #14, #25 |
| **Responsible Disclosure** | All documented findings, with program outcomes preserved |
| **Security Reporting** | Sanitized write-ups with reproduction, impact, limitations, and remediation |

### What this means in practice

The portfolio is designed to demonstrate that I can move beyond vulnerability identification:

**Recon → Hypothesis → Manual validation → Root cause → Impact → Evidence → Risk assessment → Report → Remediation**

That distinction is important in VAPT work: finding a suspicious response is only the beginning; the security value comes from proving what boundary failed and what an attacker can actually achieve.

---

## 🧪 How I Validate a Finding

My testing workflow is designed to move from **observation → proof → impact**, rather than treating scanner output as a vulnerability by itself.

```
Scope & Authorization
        ↓
Reconnaissance
        ↓
Attack Surface Mapping
        ↓
Technology / Architecture Identification
        ↓
Endpoint & API Discovery
        ↓
Security Hypothesis
        ↓
Controlled Validation
        ↓
Root-Cause Analysis
        ↓
Impact Verification
        ↓
Evidence Collection
        ↓
Risk / Severity Assessment
        ↓
Remediation Guidance
        ↓
Responsible Disclosure
```

### Validation principles

- **Reproduce before reporting**
- **Separate observation from demonstrated impact**
- **Use controlled accounts and infrastructure where possible**
- **Avoid unnecessary destructive testing**
- **Document limitations explicitly**
- **Do not claim compromise that was not demonstrated**
- **Preserve enough technical detail for a security team to reproduce the issue**
- **Sanitize private programs before public publication**

---

## 🛠️ Security Toolkit

### Web & API
Burp Suite · OWASP ZAP · Postman · SQLMap · Nuclei · ffuf · Dalfox

### Reconnaissance
Nmap · Subfinder · httpx · Katana · Amass · Shodan

### Mobile Security
MobSF · Frida

### Infrastructure & Security Testing
Nessus · Metasploit · Wireshark

> Tools are supporting capabilities. The portfolio emphasizes the reasoning and validation performed with them.

---

## 🎯 Core VAPT Areas

- Web application penetration testing
- API security testing
- Authentication and authorization testing
- Business-logic testing
- SSRF and server-side attack surface analysis
- Security configuration review
- Cloud and infrastructure exposure analysis
- Mobile application security
- Network/service enumeration
- Vulnerability validation and impact assessment
- Bug bounty research
- Responsible disclosure

---

## 📁 Repository Structure

```
mukesh-vapt/
│
├── methodology/
│   ├── vapt-methodology.md
│   ├── web-application.md
│   ├── api-security.md
│   └── reconnaissance.md
│
├── vulnerabilities/
│   ├── xss/
│   ├── sqli/
│   ├── ssrf/
│   ├── idor/
│   ├── cors/
│   ├── jwt/
│   └── request-smuggling/
│
├── bug-bounty/
│   ├── README.md
│   ├── methodology.md
│   ├── writeup-template.md
│   └── writeups/
│       ├── 01 → 25 security research write-ups
│       └── ...
│
└── README.md
```

→ [Browse the complete research index](bug-bounty/README.md)

---

## 📚 What You Will Find Here

- VAPT methodologies and checklists
- Web and API security research
- Reconnaissance workflows
- Authentication and authorization testing
- Cloud and infrastructure security research
- Mobile security research
- Sanitized bug bounty write-ups
- Root-cause analysis
- Impact validation
- Remediation guidance
- Responsible-disclosure documentation

---

## 🛡️ Responsible Disclosure & Publication Policy

This repository is intentionally conservative about what is published.

I do not publish:

- Credentials, tokens, API keys, or secrets
- Customer personal information
- Private report attachments
- Private target details when disclosure is prohibited
- Exploit material that would unnecessarily facilitate unauthorized access

Private and confidential submissions are **sanitized** while preserving the technical security lesson and the actual program outcome.

All testing is performed only against assets for which I have authorization or within intentionally vulnerable environments.

---

## 📫 Connect

- **GitHub:** [@mukeshadla21](https://github.com/mukeshadla21)
- **Security Portfolio:** This repository
- **LinkedIn:** Add your professional LinkedIn URL

---

## ⚠️ Disclaimer

The techniques and examples in this repository are provided for authorized security testing and educational purposes. Always obtain explicit permission before testing systems you do not own.

---

> **Security research is not just finding a vulnerability — it is proving the security boundary, demonstrating realistic impact, understanding the root cause, and communicating the result responsibly.**
