# Finding #4 — OAuth Client Identifier Accepted Without User Authentication

## 🐞 Bug Found on Bugcrowd

**Program:** Moneytree KK  
**Area:** API Security  
**Type:** Authentication / Authorization  
**Status:** Duplicate / Not Applicable

## What I Found

I found an API endpoint that could be accessed without logging in as a user.

The endpoint normally rejected requests without authentication. However, when I supplied the application's **public OAuth client identifier** in the `x-api-key` header, the request was accepted.

In simple terms:

> The API was accepting a public client identifier instead of requiring a user-authenticated session.

## What I Tested

| Test | Result |
|---|---|
| No authentication | HTTP 401 |
| Valid public client identifier | HTTP 200 |
| Invalid client identifier | HTTP 401 |
| Normal application flow | User authentication required |

The successful request returned financial institution information.

## Why This Is a Security Finding

An OAuth **client identifier** identifies an application. It does not prove that a user is authenticated.

The behavior I observed was:

```text
Public Client ID
      ↓
API accepts request
      ↓
No user session / Bearer token
      ↓
Data returned
```

## Impact

The endpoint could be accessed without a user-authenticated session when the public client identifier was supplied.

The returned information was institution metadata rather than customer-specific information, so the direct impact of this particular endpoint was limited.

## Bugcrowd Result

The submission was marked **Duplicate / Not Applicable**.

Bugcrowd identified it as overlapping with an earlier authentication-bypass report involving hardcoded OAuth client credentials.

The program also noted that the exposed institution information was not considered sensitive on its own.

## What I Learned

This finding strengthened my API security testing experience:

- A public OAuth client ID is **not the same as a user access token**.
- Test APIs with and without authentication.
- Check what each authentication header actually proves.
- Compare related authenticated and unauthenticated endpoints.
- Always investigate the sensitivity and real-world impact of returned data.
- A duplicate finding is still useful practical research experience.

## Simple Security Principle

> **A client ID identifies the application. A user access token authenticates the user. An API should enforce the correct authorization boundary.**

### Responsible Disclosure

The original Bugcrowd engagement was private. This public portfolio entry does not disclose the exact target URL, client identifier, submission ID, credentials, or private evidence.
