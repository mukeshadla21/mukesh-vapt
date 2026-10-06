# VAPT Methodology

## 1. Scope and Authorization

Before testing:

- Confirm written authorization.
- Identify in-scope domains, applications, APIs and IP ranges.
- Understand prohibited testing techniques.
- Record testing windows and escalation contacts.

## 2. Reconnaissance

Typical activities:

- Subdomain enumeration
- DNS analysis
- Technology fingerprinting
- Port and service discovery
- HTTP probing
- JavaScript discovery
- Endpoint and parameter discovery

## 3. Attack Surface Mapping

Document:

- Applications
- APIs
- Authentication points
- Administrative functions
- File upload functionality
- URL-fetching functionality
- Webhooks
- External integrations
- Sensitive workflows

## 4. Vulnerability Assessment

Combine automated scanning with manual validation.

Priority areas:

- Authentication
- Authorization
- Session management
- Input validation
- API authorization
- Business logic
- File handling
- Server-side request functionality
- Security configuration

## 5. Validation

For every suspected issue:

1. Reproduce the behavior.
2. Remove scanner-only assumptions.
3. Confirm security boundary crossing.
4. Determine realistic impact.
5. Capture minimal evidence.
6. Check whether the issue is reproducible.

## 6. Reporting

A professional finding should contain:

- Title
- Severity
- Affected asset
- CWE / OWASP category where applicable
- Description
- Preconditions
- Reproduction steps
- Evidence
- Security impact
- Remediation
- References

## 7. Retesting

After remediation:

- Reproduce the original test case.
- Verify the security control.
- Check for bypasses.
- Record the final status.
