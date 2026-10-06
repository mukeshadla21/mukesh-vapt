# Mukesh VAPT

> **Vulnerability Assessment & Penetration Testing | Web & API Security | Bug Bounty | Security Research**

Welcome to my security research and penetration-testing portfolio.

This repository documents my practical approach to **Vulnerability Assessment and Penetration Testing (VAPT)**, including reconnaissance, web application security, API security, vulnerability validation, security research, and responsible disclosure.

The goal is to demonstrate **practical security thinking, vulnerability validation, and responsible disclosure** rather than simply listing tools or commands.

---

## 🏆 Security Research Recognition

### RepAutomate — Security Hall of Fame Researcher

I was publicly recognized by **RepAutomate** as a security researcher in its **2026 Security Hall of Fame** for a responsible security contribution.

**Recognition details:**

- **Researcher:** Mukesh Adla
- **Recognition:** 2026 Security Hall of Fame
- **Date of contribution:** May 2026
- **Contribution:** Clickjacking vulnerability
- **Disclosure:** Responsibly reported to the security team
- **Acknowledgement:** Public recognition granted by the organization

🔗 **[View RepAutomate Security Hall of Fame](https://repautomate.co.za/security/hall-of-fame/)**

📄 **[Read the technical write-up](bug-bounty/writeups/01-clickjacking-repautomate.md)**

> This recognition demonstrates practical vulnerability research, responsible disclosure, and communication with a security team.

---

## 📊 Security Research Portfolio

| # | Research / Contribution | Status | Publication |
|---|---|---|---|
| 01 | Clickjacking — RepAutomate | Publicly acknowledged | [Technical write-up](bug-bounty/writeups/01-clickjacking-repautomate.md) |
| 02 | Clickjacking & Scope Validation | Out of Scope — confidential target | [Finding #2](bug-bounty/writeups/02-clickjacking-scope-validation.md) |
| 03 | Verbose GraphQL Error Disclosure | Informational (P5) — confidential | [Finding #3](bug-bounty/writeups/03-graphql-error-disclosure-lesson.md) |
| 04 | OAuth Client Identifier / API Authorization | Duplicate / Not Applicable | [Finding #4](bug-bounty/writeups/04-oauth-client-identifier-authentication-lesson.md) |
| 05 | CORS Application Configuration Disclosure | Low — responsible disclosure | [Finding #5](bug-bounty/writeups/05-cors-application-configuration-disclosure.md) |
| 06 | MCP Token Creation / Plan Entitlement | Entitlement inconsistency | [Finding #6](bug-bounty/writeups/06-mcp-token-entitlement.md) |
| 07 | Insufficient Authentication Rate Limiting | CWE-307 — responsible disclosure | [Finding #7](bug-bounty/writeups/07-login-rate-limiting-cwe-307.md) |
| 08 | Public Password Policy Configuration | Low — responsible disclosure | [Finding #8](bug-bounty/writeups/08-nucleus-password-policy-disclosure.md) |
| 09 | Public Management Server Status | Low — responsible disclosure | [Finding #9](bug-bounty/writeups/09-nucleus-server-status-disclosure.md) |
| 10 | Weak Content Security Policy | Informational — responsible disclosure | [Finding #10](bug-bounty/writeups/10-nucleus-csp-misconfiguration.md) |
| 11 | Hardcoded Third-Party API Key | Duplicate | [Finding #11](bug-bounty/writeups/11-alaan-hardcoded-api-key.md) |
| 12 | Unauthenticated Report Endpoint / Operational Data Exposure | Responsible disclosure | [Finding #12](bug-bounty/writeups/12-unauthenticated-report-operational-data.md) |
| 13 | Weak Password Policy / Low-Entropy Password Acceptance | Low / Informational | [Finding #13](bug-bounty/writeups/13-weak-password-policy-low-entropy.md) |
| 14 | Password Change Allows Current Password Reuse | Low / Informational | [Finding #14](bug-bounty/writeups/14-password-change-allows-current-password-reuse.md) |
| 15 | Authenticated Blind SSRF — Domain Asset Verification | Medium — CWE-918 | [Finding #15](bug-bounty/writeups/15-authenticated-blind-ssrf-domain-asset-verification.md) |

> Findings are documented according to their actual disclosure or program outcome. Confidential submissions are sanitized and private credentials or sensitive evidence are not published.

---

## 🔐 Security Focus

- Web Application Penetration Testing
- API Security Testing
- Vulnerability Assessment
- Authentication & Authorization Testing
- Business Logic Testing
- Reconnaissance & Attack Surface Discovery
- JavaScript & Endpoint Analysis
- Mobile Application Security
- Network Security
- Bug Bounty Research
- Security Research
- Responsible Disclosure

---

## 🧰 Security Toolkit

### Web & API
- Burp Suite
- OWASP ZAP
- Postman
- SQLMap
- Nuclei
- ffuf
- Dalfox

### Reconnaissance
- Nmap
- Subfinder
- httpx
- Katana
- Amass
- Shodan

### Mobile Security
- MobSF
- Frida

### Infrastructure & Security Testing
- Nessus
- Metasploit
- Wireshark

---

## 🎯 Vulnerability Research

Areas of research and testing include:

| Area | Examples |
|---|---|
| Injection | SQL Injection, XSS, Command Injection |
| Access Control | IDOR, BOLA, Privilege Escalation |
| Authentication | Password policy, session security, MFA weaknesses |
| API Security | BOLA, broken authentication, excessive data exposure |
| Server-Side | SSRF, request smuggling, file inclusion |
| Client-Side | DOM XSS, CORS, Clickjacking |
| Tokens | JWT security and implementation weaknesses |
| Business Logic | Workflow bypasses, parameter manipulation, abuse cases |
| Configuration | Security headers, exposed services, misconfiguration |

---

## 🔎 VAPT Methodology

My general assessment workflow:

```text
Scope & Authorization
        ↓
Reconnaissance
        ↓
Attack Surface Mapping
        ↓
Technology Fingerprinting
        ↓
Endpoint / API Discovery
        ↓
Automated & Manual Testing
        ↓
Vulnerability Validation
        ↓
Impact Assessment
        ↓
Evidence Collection
        ↓
Risk Rating
        ↓
Remediation Guidance
        ↓
Professional Reporting
```

Manual validation is used wherever possible to reduce false positives and establish real security impact.

---

## 🕵️ Bug Bounty Workflow

```text
Target Scope
    ↓
Subdomain Enumeration
    ↓
Live Host Discovery
    ↓
Technology Identification
    ↓
JavaScript Analysis
    ↓
Endpoint & API Discovery
    ↓
Parameter Discovery
    ↓
Vulnerability Testing
    ↓
Manual Validation
    ↓
Impact Verification
    ↓
Responsible Disclosure
```

All testing is performed only against assets for which I have permission or within intentionally vulnerable environments.

---

## 📂 Repository Structure

```text
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
│   ├── methodology.md
│   ├── writeup-template.md
│   └── writeups/
│       ├── 01-clickjacking-repautomate.md
│       ├── 02-clickjacking-scope-validation.md
│       ├── 03-graphql-error-disclosure-lesson.md
│       ├── 04-oauth-client-identifier-authentication-lesson.md
│       ├── 05-cors-application-configuration-disclosure.md
│       ├── 06-mcp-token-entitlement.md
│       ├── 07-login-rate-limiting-cwe-307.md
│       ├── 08-nucleus-password-policy-disclosure.md
│       ├── 09-nucleus-server-status-disclosure.md
│       ├── 10-nucleus-csp-misconfiguration.md
│       ├── 11-alaan-hardcoded-api-key.md
│       ├── 12-unauthenticated-report-operational-data.md
│       └── 13-weak-password-policy-low-entropy.md
│
└── README.md
```

---

## 📚 What You Will Find Here

This repository contains:

- VAPT checklists
- Testing methodologies
- Reconnaissance workflows
- Verified vulnerability research
- Sanitized security write-ups
- API security testing techniques
- Lab exercises
- False-positive analysis
- Remediation guidance
- Responsible-disclosure practices

---

## 🛡️ Responsible Disclosure

Security research in this repository is intended for:

- Authorized penetration testing
- Bug bounty programs within their published scope
- Intentionally vulnerable applications
- Security training labs
- Defensive security research

I do not publish credentials, private information, undisclosed sensitive vulnerabilities, or material intended to facilitate unauthorized access.

---

## 📫 Connect

- **GitHub:** [@mukeshadla21](https://github.com/mukeshadla21)
- **Security Portfolio:** This repository
- **LinkedIn:** Add your professional LinkedIn URL

---

## ⚠️ Disclaimer

The techniques and examples in this repository are provided for authorized security testing and educational purposes. Always obtain explicit permission before testing systems you do not own.

---

### 🔐 Security is not just finding vulnerabilities — it is proving impact, communicating risk, and helping organizations fix security issues.
