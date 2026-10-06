# Finding #8 — Public Disclosure of Password Policy Configuration

## 🐞 Bug Found — Responsible Disclosure

**Program:** Nucleus Software  
**Area:** Umbraco Management API  
**Vulnerability:** Information Disclosure  
**Severity:** Low

## What I Found

A publicly accessible Umbraco management API endpoint disclosed password policy configuration without authentication.

The response exposed settings such as minimum password length and password-complexity requirements.

## Impact

The information does not expose credentials or user data, but it provides attackers with additional information about the administrative authentication configuration and can support reconnaissance.

## Recommended Remediation

- Require authentication before returning password policy configuration.
- Restrict management APIs to authorized administrative users.
- Avoid exposing internal security configuration publicly.

### Responsible Disclosure

The portfolio entry intentionally omits unnecessary sensitive evidence.
