# Finding #6 — MCP Token Creation Available to Basic Plan Users

## 🐞 Bug Found — Responsible Disclosure

**Program:** AuditWard  
**Area:** API Security / Entitlement Enforcement  
**Vulnerability:** Improper Authorization / Entitlement Enforcement  
**Observed Impact:** Limited  
**Status:** MCP usage remained blocked

## What I Found

I found that a user on the Basic plan could create an MCP token through the API even though MCP access was a paid feature available on higher subscription plans.

The API successfully created the credential and returned **HTTP 201 Created**.

However, using the generated token against the MCP service was still rejected with **HTTP 403** because the subscription did not include MCP access.

## Security Impact

The behavior represents an entitlement-enforcement inconsistency:

- A Basic-plan user could create credentials for a paid feature.
- The generated credential could not be used to access MCP functionality during testing.
- I did not identify a method to bypass the actual MCP subscription restriction.

## Expected Behavior

Users whose plan does not include MCP should be prevented from creating MCP credentials.

## Recommended Remediation

- Validate subscription entitlement before token creation.
- Prevent Basic-plan users from generating MCP credentials.
- Apply the same entitlement check consistently at credential creation and feature-use stages.

### Responsible Disclosure

This entry intentionally excludes private credentials, tokens, submission identifiers, and other sensitive evidence.
