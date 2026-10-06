# Finding #7 — Insufficient Authentication Rate Limiting

## 🐞 Bug Found — Responsible Disclosure

**Program:** WebinarGeek  
**Area:** Authentication  
**Vulnerability:** Improper Restriction of Excessive Authentication Attempts  
**CWE:** CWE-307  
**Severity:** Potentially Medium

## What I Found

During authorized testing, I evaluated the login endpoint for protections against repeated failed authentication attempts.

After several hundred failed attempts using test credentials, I did not observe authentication-specific controls such as account lockout, CAPTCHA, progressive delays, HTTP 429 responses, or Retry-After headers.

## Testing Performed

Testing included:

- Sequential failed login attempts
- Repeated attempts against a test account
- Concurrent authentication requests
- Different User-Agent values
- More than 300 failed authentication attempts

No third-party accounts were targeted and no account compromise was attempted.

## Impact

Insufficient authentication throttling may increase exposure to:

- Credential stuffing
- Password spraying
- Online brute-force attacks

Infrastructure-level connection limiting was observed during high-concurrency testing, but it did not appear to provide equivalent protection against repeated sequential authentication failures.

## Recommended Remediation

Consider:

- Account- and IP-based authentication throttling
- Progressive delays
- CAPTCHA or equivalent human verification
- Temporary lockout or risk-based controls
- Monitoring and alerting for excessive failures
- Appropriate 429 / Retry-After responses where applicable

### Responsible Disclosure

The testing was limited to authorized authentication testing with test credentials. No accounts were compromised.
