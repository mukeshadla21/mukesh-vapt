# Security Research Lesson #4 — Public OAuth Client Identifiers and Authorization Boundaries

## Topic

**API authorization testing: public OAuth client identifiers vs. user authentication**

## Research Context

During authorized API security research against a managed bug bounty program, I identified an API endpoint where a publicly available OAuth client identifier was sufficient to obtain a dataset without a user Bearer token.

The submission was ultimately marked **Not Applicable / Duplicate**. The program linked it to an earlier P1 report covering the broader authentication-bypass issue.

## What I Learned

A public OAuth client identifier should not automatically be treated as proof of user authentication.

When assessing API authorization, distinguish between:

- **Client identification** — identifying which application/client is making a request.
- **User authentication** — proving the identity of a user.
- **Authorization** — determining what the authenticated principal is allowed to access.

A client identifier embedded in a public application may be intentionally exposed. Its presence alone should therefore not be considered a secret credential.

## API Testing Approach

For an endpoint that appears to require authentication, compare controlled requests such as:

| Test | Security Question |
|---|---|
| No authentication | Is the endpoint publicly accessible? |
| Public client identifier only | Is client identification being treated as authentication? |
| Invalid client identifier | Is the client identifier actually validated? |
| Valid user access token | What does the intended authenticated flow look like? |
| Different user/session context | Is authorization enforced consistently? |

The objective is to determine whether the server is enforcing the correct **authentication and authorization boundary**, not merely whether a header is required.

## Important Bug Bounty Lesson

This submission was marked duplicate because another researcher had already reported the broader authentication-bypass condition.

The program also noted that the institution metadata itself was not considered sensitive on its own and encouraged demonstrating how the weakness could be used to attack other services.

This reinforced an important research principle:

> **A technically interesting authentication difference is not necessarily a unique or rewardable vulnerability. Establish distinct impact and check for existing disclosures before treating it as a standalone finding.**

## What I Would Improve in Future Testing

1. Map the authentication boundary before reporting.
2. Determine whether the exposed data is actually sensitive.
3. Compare related authenticated and unauthenticated endpoints.
4. Investigate whether the same authorization weakness affects more sensitive operations.
5. Look for a distinct security impact rather than reporting only an authentication inconsistency.
6. Check program scope and known disclosures where available.

## Responsible Disclosure

The original submission concerned a private bug bounty program. This repository intentionally does not publish the target URL, submission ID, client identifier, raw API responses, credentials, private evidence, or other program-specific sensitive details.

This page documents the **general API security lesson and research methodology only**.

---

### Research Principle

> **Do not confuse client identification with user authentication — and do not stop at an authentication difference when the real objective is to prove security impact.**
