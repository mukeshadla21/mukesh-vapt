# Finding #24 — Production Authentication Origin Directly Reachable Outside Cloudflare

## 🐞 Bug Found — Responsible Disclosure / Bug Bounty

**Program:** Banco Plata  
**Report:** #4006405  
**Area:** Infrastructure / Web Application Security  
**Vulnerability:** Public Origin Exposure / Security Control Bypass  
**Affected Asset:** `auth-prime.platacard.mx`  
**Program Outcome:** Duplicate (#3620121) — original report assessed as Informative

## What I Found

I identified that the production authentication service behind the in-scope `auth-prime.platacard.mx` hostname was directly reachable from the public Internet through AWS origin infrastructure.

The DNS relationship exposed a public origin path through:

```text
auth-prime.platacard.mx
        |
        v
auth.prime.diftech.net
        |
        v
Public AWS infrastructure
```

The exposed service responded with `server: istio-envoy` and processed the production authentication API.

This was not simply an unrelated web server. The public origin returned structured responses from the production authentication application.

## Validation

I compared benign requests through the intended production hostname and directly against the discovered origin.

A benign authentication-flow request to the Cloudflare-protected authentication service produced a Cloudflare block response.

The same type of benign request sent through the affected `auth-prime.platacard.mx` route reached the authentication application and returned an application-level response indicating that the supplied PAR identifier was expired or not found.

I then forced the hostname to resolve to the discovered AWS origin. The origin returned the same application-level response, confirming that the public origin was serving the production authentication application.

Additional authentication-flow endpoints also returned structured application responses, including validation and authentication-flow errors. Testing was limited to benign requests and a nonexistent/expired PAR identifier.

## Security Impact

The primary issue is **direct public reachability of a production authentication origin outside the intended Cloudflare edge path**.

If Cloudflare is intended to act as a security boundary, direct origin access can allow an attacker who discovers the origin to send traffic without passing through controls enforced exclusively at Cloudflare, such as:

- WAF rules.
- Bot-management controls.
- IP reputation filtering.
- Edge-based rate limiting.
- Other Cloudflare-specific abuse protections.

The exposed service handles production authentication functionality, including login initiation, authentication challenges, authorization, and token-related flows.

I did **not** attempt:

- OTP brute forcing.
- Credential stuffing.
- Sending OTPs to customers.
- Customer account access.
- Credential attacks.
- Service degradation.

No customer accounts or data were accessed.

## Expected Behavior

If Cloudflare is intended to be the external security boundary, the production authentication origin should not be directly reachable from the public Internet.

The origin should accept production traffic only through the intended trusted edge/infrastructure path.

## Recommended Remediation

- Restrict public access to the production authentication origin.
- Allow inbound traffic only from the intended Cloudflare/edge infrastructure where applicable.
- Review AWS security groups and load-balancer/ingress configuration.
- Review Istio authorization policies protecting the authentication service.
- Apply appropriate abuse protection and rate limiting at the origin as defense in depth.
- Review other production origin records for the same exposure pattern.

## Program Outcome

Banco Plata closed the report as **Duplicate (#3620121)**.

The program stated that the report matched an issue previously reported and assessed as **Informative**.

Although the report was a duplicate and received no separate credit, this portfolio records the **actual infrastructure exposure independently identified and reported**, including validation that the public origin served the production authentication application.

### Responsible Disclosure

This public entry is sanitized and does not reproduce private report evidence, unnecessary origin infrastructure details, authentication data, credentials, or attack attempts against customer accounts.
