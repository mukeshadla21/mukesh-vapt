# Finding #23 — Public Firebase Storage Bucket Allows Anonymous Enumeration and File Download

## 🐞 Bug Found — Responsible Disclosure / Bug Bounty

**Program:** Unico IDtech  
**Report:** #3810635  
**Area:** Cloud Storage / Access Control  
**Vulnerability:** Publicly Accessible Firebase Storage Bucket / Missing Access Control  
**Program Outcome:** Duplicate (#3515437) — original report assessed as Informative

## What I Found

I identified a Firebase Storage bucket associated with Unico/Acesso infrastructure that was accessible without authentication.

The exposed development bucket allowed an unauthenticated internet user to:

- Enumerate stored objects.
- Download stored files.
- Access application artifacts such as Android APK files.
- Access development-related resources.

The bucket identified during testing was:

`acesso-unico-dev.appspot.com`

The storage API returned bucket contents without requiring an authentication credential.

## Proof of Concept

A sanitized request to the Firebase Storage API demonstrated anonymous enumeration:

```http
GET /v0/b/acesso-unico-dev.appspot.com/o HTTP/1.1
Host: firebasestorage.googleapis.com
```

The request returned:

```text
HTTP 200 OK
```

The response contained a list of objects stored in the bucket.

I also verified that an Android APK stored in the bucket could be downloaded without authentication.

The downloaded application package was associated with:

```text
br.com.appunico
```

No authentication cookie, API key, bearer token, or other user credential was required for the tested access.

## Additional Production Verification

After the initial submission, I identified that the production Firebase Storage bucket was also publicly accessible:

`acesso-unico-prod.appspot.com`

Anonymous access was verified against multiple production objects, including application configuration and theme resources.

The retrieved configuration contained application-related metadata such as deep-link definitions, routes, feature metadata, and references to additional storage objects.

No production credentials, authentication secrets, or verified customer records were identified during testing.

## Impact

The demonstrated issue was unauthorized public access to Firebase Storage contents.

Potential security impact included:

- Enumeration of stored storage objects.
- Unauthorized download of files intended to be access-controlled.
- Exposure of Android application artifacts.
- Exposure of development-related resources and configuration files.
- Increased reconnaissance and reverse-engineering opportunities against the associated application ecosystem.

The program later stated that the disclosed files were considered non-sensitive and that the bucket was intentionally configured for public site assets.

Therefore, the impact should be understood primarily as **public storage exposure / access-control misconfiguration**, rather than confirmed exposure of sensitive customer information.

## Recommended Remediation

- Review Firebase Storage Security Rules.
- Restrict bucket listing to authorized users or trusted services where listing is not intentionally public.
- Restrict object reads to the intended audience.
- Review all objects currently accessible anonymously.
- Separate intentionally public assets from private application/development resources.
- Ensure development and production buckets follow appropriate access-control policies.

## Program Outcome

Unico IDtech closed the report as **Duplicate (#3515437)**.

The program confirmed that the same development bucket, endpoint, and APK had already been reported and assessed.

The original report was evaluated as **Informative**, with the program stating that the disclosed files were not considered sensitive and that the bucket was intentionally configured for public site assets.

Although the submission was a duplicate and did not receive separate credit, this portfolio records the **actual issue independently identified and reported**, including the additional production-bucket verification.

### Responsible Disclosure

This public entry is sanitized. Private report identifiers, attachments, and unnecessary storage object details are not reproduced beyond what is needed to explain the finding.
