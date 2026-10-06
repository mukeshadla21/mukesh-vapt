# Finding #9 — Public Management Server Status Endpoint

## 🐞 Bug Found — Responsible Disclosure

**Program:** Nucleus Software  
**Area:** Umbraco Management API  
**Vulnerability:** Information Disclosure / Missing Authentication  
**Severity:** Low

## What I Found

An Umbraco management API endpoint was publicly accessible without authentication and returned the runtime status of the server.

## Impact

Although the disclosed status information is limited, it confirms that the management interface is active and provides additional information that can support reconnaissance and attack-surface mapping.

## Recommended Remediation

- Require authentication for management status endpoints.
- Restrict management APIs to authorized administrators or trusted networks.
- Return 401 or 403 to unauthenticated requests.

### Responsible Disclosure

The portfolio entry intentionally omits unnecessary sensitive evidence.
