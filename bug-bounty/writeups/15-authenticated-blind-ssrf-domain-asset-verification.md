# Finding #15 — Authenticated Blind SSRF via Domain Asset Verification

## 🐞 Bug Found — Responsible Disclosure

**Program:** Dashten  
**Area:** Web Application / Domain Asset Verification  
**Vulnerability:** Server-Side Request Forgery (SSRF)  
**CWE:** CWE-918  
**Severity:** Medium (assessment)

## What I Found

During authenticated testing of the Domain Asset Verification functionality, I found that the application performs server-side HTTPS requests to a hostname supplied by the authenticated user.

The verification workflow accepted a hostname resolving to a link-local address and initiated an outbound request from the application server.

The response body was not returned to the client, making this a **blind SSRF** condition. However, the request produced a consistent response-time difference compared with normal verification requests, providing evidence that the server was attempting to connect to the supplied destination.

## How It Worked

1. Authenticated to the application using my own test account.
2. Created a Domain asset through the authorized asset-management workflow.
3. Supplied a controlled hostname that resolved to a link-local address.
4. Triggered the domain verification functionality.
5. Observed that the application server initiated the verification request.
6. Compared the response timing with normal verification requests.
7. The controlled test produced a significantly longer response time (approximately 15.7 seconds), consistent with a server-side connection attempt.
8. Deleted the temporary test asset after validation.

The exact target URL and detailed request data are intentionally omitted from this public write-up.

## Security Impact

If an attacker can influence destinations contacted by the server, SSRF can potentially be used to:

- Trigger outbound requests from the application infrastructure.
- Probe private, loopback, or link-local network ranges where network controls permit access.
- Perform limited internal network reconnaissance through observable response behavior.
- Potentially reach internal services or cloud metadata endpoints if additional application or network protections are absent.

No internal credentials, customer data, or sensitive metadata were accessed or exfiltrated during testing.

## Validation Scope

Testing was deliberately limited:

- Only my own authenticated account was used.
- No customer accounts or data were accessed.
- No destructive or disruptive requests were performed.
- No internal credentials or metadata were retrieved.
- The temporary test asset was removed after validation.

## Recommended Remediation

- Treat user-supplied hostnames as untrusted destinations.
- Resolve hostnames and validate the resulting IP addresses before making outbound requests.
- Block private, loopback, link-local, multicast, and other non-public address ranges.
- Re-check the destination after DNS resolution to mitigate DNS-rebinding scenarios.
- Restrict outbound network access from the application server using egress firewall controls.
- Use an allowlist of permitted protocols, ports, and destinations where possible.
- Avoid returning internal response data to the client.
- Log and monitor unusual outbound verification requests.

### Responsible Disclosure

This finding is documented using sanitized details. Exact target-specific request data, internal infrastructure information, and sensitive evidence are intentionally excluded from the public portfolio.
