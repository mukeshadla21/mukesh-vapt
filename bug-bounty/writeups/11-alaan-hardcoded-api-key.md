# Finding #11 — Hardcoded Third-Party API Key Exposed in Production JavaScript

## 🐞 Bug Found — Responsible Disclosure

**Program:** Alaan  
**Area:** Web Application / Client-Side Security  
**Vulnerability:** Hardcoded Credentials / Sensitive Information Disclosure  
**CWE:** CWE-798 — Use of Hard-coded Credentials  
**Suggested Severity:** High  
**Result:** Duplicate

## What I Found

I identified an active third-party **ipstack API credential** embedded directly in a production JavaScript bundle served to application users.

Because the credential was delivered to the browser, it could be extracted and reused independently of the application.

## Validation

During authorized validation, the exposed credential successfully authenticated requests to the third-party API.

I limited testing to validating the credential and did not attempt to access Alaan customer data, bypass authentication, modify application data, or perform disruptive testing.

## Potential Impact

An exposed third-party credential could allow unauthorized use of the associated service, including:

- Consumption of API quota
- Unauthorized requests against the provider
- Potential additional provider costs
- Reuse of the credential outside the intended application

## Recommended Remediation

1. Rotate the exposed credential.
2. Remove secrets from client-side JavaScript.
3. Proxy authenticated third-party requests through backend infrastructure.
4. Monitor the credential for unauthorized usage.
5. Add secret scanning and CI/CD controls to prevent credentials from entering production bundles.

## Result

The Alaan Security Team confirmed that the issue was already known and reported by another researcher. The submission was therefore marked **Duplicate**.

### Responsible Disclosure

This public write-up does not include the actual API credential or unnecessary private evidence.
