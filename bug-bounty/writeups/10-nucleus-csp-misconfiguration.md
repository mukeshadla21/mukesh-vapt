# Finding #10 — Weak Content Security Policy Configuration

## 🐞 Bug Found — Responsible Disclosure

**Program:** Nucleus Software  
**Area:** Web Application Security  
**Vulnerability:** Content Security Policy Misconfiguration  
**Severity:** Informational

## What I Found

The application Content Security Policy permitted:

`unsafe-inline`  
`unsafe-eval`

These directives reduce the protection normally provided by CSP against client-side script injection.

## Impact

I did not identify an exploitable XSS vulnerability during testing.

However, if an XSS vulnerability were introduced, the permissive CSP would provide weaker defense-in-depth protection.

## Recommended Remediation

- Remove `unsafe-inline` where practical.
- Remove `unsafe-eval`.
- Use nonce- or hash-based script controls.
- Restrict external script sources to those required by the application.

### Responsible Disclosure

This entry records the security-hardening observation without claiming an XSS vulnerability was demonstrated.
