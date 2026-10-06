# Clickjacking Vulnerability — RepAutomate

## Overview

A **Clickjacking / UI Redressing** vulnerability was identified on the RepAutomate web application during authorized security testing performed in accordance with the application's Vulnerability Disclosure Program.

The affected application could be embedded inside an attacker-controlled iframe because anti-clickjacking protections were not implemented.

## Affected Asset

**URL:** https://repautomate.co.za/

## Vulnerability Classification

- **Vulnerability:** Clickjacking / UI Redressing
- **OWASP Category:** Security Misconfiguration / related client-side attack surface
- **CWE:** CWE-1021 — Improper Restriction of Rendered UI Layers or Frames
- **Primary Controls:** `X-Frame-Options` and/or CSP `frame-ancestors`

## Technical Description

The application was observed to allow the target page to be rendered inside an external iframe.

The expected anti-clickjacking controls were not effectively preventing framing, such as:

- `X-Frame-Options`
- Content Security Policy `frame-ancestors`

This behavior can enable a malicious website to place the legitimate application underneath deceptive interface elements and attempt to trick a victim into interacting with the framed application.

## Reproduction

1. Create an HTML page containing an iframe pointing to the affected application.
2. Open the HTML page in a browser.
3. Observe that the application loads successfully inside the iframe.
4. A malicious page could potentially position deceptive UI elements over the framed application to induce unintended user interaction.

### Sanitized Proof of Concept

~~~html
<!DOCTYPE html>
<html>
<head>
    <title>Clickjacking PoC</title>
</head>
<body>
    <iframe
        src="https://repautomate.co.za/"
        width="1200"
        height="800">
    </iframe>
</body>
</html>
~~~

The PoC is provided only to demonstrate the framing behavior. Any further exploitation should be performed only within the authorized scope of the relevant security program.

## Security Impact

If exploitable in a sensitive workflow, clickjacking can allow an attacker to:

- Deceive users into interacting with application controls.
- Overlay misleading UI elements over legitimate application functionality.
- Potentially trigger unintended actions performed by an authenticated victim.

The practical impact depends on which sensitive actions are available to an authenticated user and whether those actions can be successfully induced through the framed interface.

## Recommended Remediation

Implement explicit anti-clickjacking controls.

### Recommended CSP

Prefer a Content Security Policy such as:

~~~http
Content-Security-Policy: frame-ancestors 'self';
~~~

If the application does not need to be framed by any other site, a stricter policy can be used according to the application's requirements.

### Legacy Protection

Where appropriate, also consider:

~~~http
X-Frame-Options: SAMEORIGIN
~~~

CSP `frame-ancestors` should be treated as the modern control for framing restrictions.

## Researcher Recognition

The issue was responsibly disclosed to the application/security team.

The researcher was subsequently invited to be included in the organization's **Hall of Fame** in recognition of the security contribution.

**Researcher:** Mukesh Adla

**Public acknowledgement:** Permission was granted to publicly acknowledge the contribution.

## Disclosure

This write-up is intended to document the security research methodology and the vulnerability at a high level. Sensitive information, credentials, private data, and unnecessary exploitation details are intentionally excluded.

---

**Researcher:** Mukesh Adla  
**Focus:** VAPT | Web Application Security | Bug Bounty
