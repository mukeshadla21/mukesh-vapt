# Mukesh VAPT

> **Vulnerability Assessment & Penetration Testing | Web & API Security | Bug Bounty | Security Automation**

Welcome to my security research and penetration-testing portfolio.

This repository documents my practical approach to **Vulnerability Assessment and Penetration Testing (VAPT)**, including reconnaissance, web application security, API security, vulnerability validation, security automation, and responsible disclosure.

The goal is to demonstrate **methodology and practical security thinking** rather than simply listing tools or commands.

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
- Security Automation
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
- JADX
- apktool
- ADB

### Infrastructure & Security Testing
- Nessus
- Metasploit
- Wireshark

### Automation
- Python
- Bash
- Java

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
│   └── writeup-template.md
│
├── automation/
│   └── README.md
│
└── README.md
```

---

## 📚 What You Will Find Here

This repository will contain:

- VAPT checklists
- Testing methodologies
- Reconnaissance workflows
- Vulnerability research
- Sanitized security write-ups
- API security testing techniques
- Automation scripts
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

### Security is not just finding vulnerabilities — it is proving impact, reducing risk, and communicating the fix.
