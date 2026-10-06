# Bug Bounty & Security Research Index

This directory contains sanitized write-ups from vulnerability research and responsible-disclosure work.

## How to Read the Portfolio

The entries are intentionally separated into three signals:

1. **What was technically found** — the security weakness and validated behavior.
2. **What impact was demonstrated** — the security consequence actually established during testing.
3. **What the program decided** — accepted, duplicate, informative, out of scope, or another final classification.

A duplicate or informative result is **not rewritten as a successful bounty**. The finding remains documented because independent discovery, validation quality, technical reasoning, and accurate reporting are useful indicators of security research ability.

---

## ⭐ Start Here — Highest Technical Signal

| Finding | Why it stands out |
|---|---|
| [#16 — cardano-submit-api DoS](writeups/16-cardano-submit-api-unbounded-request-body-dos.md) | High-severity resource exhaustion with source-level root-cause analysis |
| [#17 — ouroboros-consensus TOCTOU](writeups/17-ouroboros-consensus-toctou-copytoimmutabledb-node-crash.md) | Concurrency/state-race reasoning with deterministic reproduction |
| [#15 — Authenticated Blind SSRF](writeups/15-authenticated-blind-ssrf-domain-asset-verification.md) | Server-side request behavior validated against controlled infrastructure |
| [#20 — Dovecot Stats Crash](writeups/20-dovecot-stats-unauthenticated-malformed-chunked-crash.md) | Unauthenticated protocol edge case leading to worker crash and telemetry reset |
| [#25 — MDVM Lifecycle / Revocation](writeups/25-mdvm-deletion-fails-wpb-rwsca-pns-revocation.md) | Cross-service authorization lifecycle failure with fresh-token validation |
| [#24 — Production Origin Exposure](writeups/24-banco-plata-cloudflare-origin-auth-exposure.md) | Cloud edge/origin trust-boundary analysis |
| [#22 — OTel Telemetry Injection](writeups/22-crypto-com-unauthenticated-otel-telemetry-injection.md) | Unauthenticated observability ingestion and telemetry-integrity analysis |

---

## 📚 Complete Index

| # | Finding | Primary security area | Outcome |
|---|---|---|---|
| 01 | [Clickjacking — RepAutomate](writeups/01-clickjacking-repautomate.md) | Client-side / Web | Public Hall of Fame recognition |
| 02 | [Clickjacking & Scope Validation](writeups/02-clickjacking-scope-validation.md) | Client-side / Web | Out of Scope |
| 03 | [Verbose GraphQL Error Disclosure](writeups/03-graphql-error-disclosure-lesson.md) | API / Information Disclosure | Informational |
| 04 | [OAuth Client Identifier / API Authorization](writeups/04-oauth-client-identifier-authentication-lesson.md) | Authentication / API | Duplicate / N/A |
| 05 | [CORS Application Configuration Disclosure](writeups/05-cors-application-configuration-disclosure.md) | Web / Configuration | Low |
| 06 | [MCP Token Creation / Plan Entitlement](writeups/06-mcp-token-entitlement.md) | Authorization / Entitlement | Entitlement inconsistency |
| 07 | [Insufficient Authentication Rate Limiting](writeups/07-login-rate-limiting-cwe-307.md) | Authentication | CWE-307 |
| 08 | [Public Password Policy Configuration](writeups/08-nucleus-password-policy-disclosure.md) | Information Disclosure | Low |
| 09 | [Public Management Server Status](writeups/09-nucleus-server-status-disclosure.md) | Information Disclosure | Low |
| 10 | [Weak Content Security Policy](writeups/10-nucleus-csp-misconfiguration.md) | Web / Configuration | Informational |
| 11 | [Hardcoded Third-Party API Key](writeups/11-alaan-hardcoded-api-key.md) | Secrets / Client-side | Duplicate |
| 12 | [Unauthenticated Report Endpoint / Operational Data Exposure](writeups/12-unauthenticated-report-operational-data.md) | API / Access Control | Responsible disclosure |
| 13 | [Weak Password Policy / Low-Entropy Password Acceptance](writeups/13-weak-password-policy-low-entropy.md) | Authentication | Low / Informational |
| 14 | [Password Change Allows Current Password Reuse](writeups/14-password-change-allows-current-password-reuse.md) | Authentication | Low / Informational |
| 15 | [Authenticated Blind SSRF](writeups/15-authenticated-blind-ssrf-domain-asset-verification.md) | Server-side / SSRF | Medium / CWE-918 |
| 16 | [Unbounded Request Body DoS — cardano-submit-api](writeups/16-cardano-submit-api-unbounded-request-body-dos.md) | Availability / API | High / CVSS 7.5 |
| 17 | [TOCTOU Race — ouroboros-consensus](writeups/17-ouroboros-consensus-toctou-copytoimmutabledb-node-crash.md) | Concurrency / Availability | High / CVSS 7.5 |
| 18 | [FILTER SIEVE SCRIPT Worker Crash — Dovecot](writeups/18-dovecot-filter-sieve-script-worker-crash.md) | Protocol / Availability | Informative |
| 19 | [doveadm uint32 Validation Crash — Dovecot](writeups/19-dovecot-doveadm-uint32-validation-worker-crash.md) | Input Validation / Availability | Informative |
| 20 | [Unauthenticated Stats Service Crash — Dovecot](writeups/20-dovecot-stats-unauthenticated-malformed-chunked-crash.md) | HTTP / Availability | Remote crash / telemetry loss |
| 21 | [Vercel Sandbox Privilege Boundary Escalation](writeups/21-vercel-sandbox-proc1-mem-auth-bypass-root.md) | Sandbox / Privilege Boundary | Not Applicable / Out of Scope |
| 22 | [Unauthenticated OTel Telemetry Injection — Crypto.com](writeups/22-crypto-com-unauthenticated-otel-telemetry-injection.md) | Observability / API | Duplicate |
| 23 | [Public Firebase Storage — Unico IDtech](writeups/23-unico-public-firebase-storage-enumeration-download.md) | Cloud Storage / Access Control | Duplicate / Informative |
| 24 | [Production Authentication Origin Exposure — Banco Plata](writeups/24-banco-plata-cloudflare-origin-auth-exposure.md) | Cloud / Infrastructure | Duplicate / Informative |
| 25 | [MDVM Deletion / Wallet Revocation Failure](writeups/25-mdvm-deletion-fails-wpb-rwsca-pns-revocation.md) | Authentication Lifecycle | Duplicate |

---

## 🧩 Research Categories

### Authentication & Authorization
#4 · #6 · #7 · #13 · #14 · #25

### Server-Side & API Security
#3 · #12 · #15 · #16 · #18 · #19 · #20 · #22

### Cloud & Infrastructure
#21 · #23 · #24

### Web & Client Security
#1 · #2 · #5 · #10 · #11

### Availability / DoS / Concurrency
#16 · #17 · #18 · #19 · #20

### Observability & Telemetry Integrity
#20 · #22

---

## 🧭 Research Quality Signals

The write-ups prioritize:

- Reproducible technical observations
- Clear security-boundary identification
- Manual validation instead of relying only on scanners
- Root-cause analysis where evidence supports it
- Demonstrated impact rather than speculative compromise
- Explicit testing limitations
- Controlled accounts and test infrastructure
- Accurate program outcomes
- Sanitization of confidential targets and evidence

---

## 🔐 Publication Standard

Private bug bounty reports are sanitized before publication.

The public repository does not intentionally publish:

- Credentials or authentication tokens
- API keys or secrets
- Customer data
- Private report attachments
- Prohibited private-target details
- Unnecessary exploit-enabling material

The purpose of each write-up is to demonstrate **security reasoning and validation quality** without exposing information that should remain confidential.
