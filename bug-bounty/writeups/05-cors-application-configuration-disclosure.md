# Finding #5 — Cross-Origin Readable Application Configuration Disclosure via CORS

## 🐞 Bug Found — Responsible Disclosure

**Program:** Cypherdote Support  
**Area:** Web Application Security  
**Vulnerability:** CORS / Information Disclosure  
**Severity:** Low

## What I Found

I found an application endpoint that returned:

`Access-Control-Allow-Origin: *`

This allowed JavaScript running from another origin to read the endpoint response.

The response contained application bootstrap and configuration information.

## What Was Exposed

The response included information such as:

- Application settings
- Administrative route references
- Permission names
- Feature configuration
- Upload configuration metadata
- Guest-role permissions
- Application version information

Examples of administrative route references included settings, team management, custom pages, logs, and file-management functionality.

## How It Worked

The behavior was:

```text
Request from another origin
        ↓
Application returns Access-Control-Allow-Origin: *
        ↓
Browser allows cross-origin JavaScript to read the response
        ↓
Application configuration is exposed
```

## Impact

The finding primarily increased the application's information exposure and reconnaissance surface.

An attacker could use the exposed configuration to:

- Fingerprint the application
- Enumerate application functionality
- Identify administrative routes
- Understand permissions and features
- Support further security testing

The finding did not by itself demonstrate access to protected user data.

## Result

The issue was responsibly disclosed to the security team and reported as **Low severity**.

## Recommended Remediation

- Restrict CORS to trusted origins where cross-origin access is actually required.
- Review whether the exposed bootstrap configuration is intended to be publicly readable.
- Remove unnecessary administrative route and configuration metadata from public responses.

### Responsible Disclosure

This portfolio entry summarizes the finding without publishing unnecessary private evidence or sensitive submission details.
