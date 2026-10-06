# Finding #12 — Unauthenticated Report Endpoint Exposes Operational Service Data

## 🐞 Bug Found — Responsible Disclosure

**Program:** Neuron7  
**Area:** API Security / Access Control  
**Vulnerability:** Missing Authentication / Unauthorized Data Exposure  
**Severity:** High potential impact

## What I Found

I identified a publicly accessible **Pre-Dispatch Summary API** endpoint that generated detailed operational reports without requiring authentication or authorization.

The endpoint accepted service-record information and returned an AI-assisted operational report.

## Information Exposed

The generated response could contain operational information including:

- Machine and equipment profiles
- Executive summaries
- Predicted root causes
- Recommended repair actions
- Reliability metrics
- Historical repair information
- Engineer checklists
- Parts recommendations
- Operational analytics
- Health scores
- AI-generated troubleshooting guidance

During testing, historical service and operational information was observed in responses. Sensitive examples are intentionally omitted from this public write-up.

## Security Impact

If the endpoint is intended for authenticated field engineers or internal support personnel, an unauthenticated Internet user could potentially obtain operational intelligence without a valid session.

An attacker could potentially:

1. Discover the exposed API.
2. Submit report-generation requests.
3. Obtain operational information without authentication.
4. Use historical service and maintenance information for reconnaissance.
5. Potentially automate requests if additional access controls are not present.

## Recommended Remediation

- Require authentication before report generation.
- Enforce authorization for the requested service records.
- Return only information the authenticated user is authorized to access.
- Review AI-generated responses for unnecessary sensitive data exposure.
- Implement monitoring and rate limiting for report-generation requests.

### Responsible Disclosure

This entry intentionally excludes sensitive service records, private evidence, credentials, and unnecessary target-specific operational information.
