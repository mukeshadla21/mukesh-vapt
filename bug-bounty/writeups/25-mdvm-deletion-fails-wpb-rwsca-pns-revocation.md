# Finding #25 — MDVM Account Deletion Does Not Revoke Associated Wallet Capabilities

## 🐞 Bug Found — Responsible Disclosure / Bug Bounty

**Program:** d-you App & German EUDI Wallet Ecosystem  
**Report:** #4047901  
**Area:** Wallet Backend / Authentication & Authorization / Lifecycle Management  
**Vulnerability:** Incomplete Account Deletion / Revocation Failure  
**Program Outcome:** Duplicate (#4046235)

## What I Found

I identified that deleting an MDVM wallet account did not fully revoke the security-sensitive capabilities associated with that wallet.

After calling:

`POST /v1/mdvm/deleteAccount`

the MDVM/device account was removed, but associated WPB, RWSCA, and PNS state remained active.

Using controlled test accounts on the local CI-test deployment, I verified that security-sensitive operations continued to work after deletion.

## RWSCA Remains Usable After Deletion

After deleting the MDVM account, the previously issued MDVM token could still operate the associated RWSCA account.

The following operations continued to return successful responses:

- PIN initialization
- `startPinSession`
- `createKeys`
- `signData`

Most importantly, `signData` produced a fresh cryptographically valid ECDSA signature after the MDVM account had already been deleted.

## Fresh MDVM Registration Still Reaches the Old RWSCA Account

The issue was not limited to stale-token acceptance.

After deleting the original MDVM account, I re-registered MDVM using the same authentication key.

The newly registered wallet received:

- A new MDVM token.
- A new `wi_id`.

Despite having a different wallet identifier and newly issued token, the new wallet could still operate the previously surviving RWSCA account, including generating a fresh valid ECDSA signature.

This demonstrated that the authorization relationship was not correctly tied to the lifecycle of the deleted MDVM account.

## WPB Remains Active

I also verified that the associated WPB account survived MDVM deletion.

After deleting the MDVM account:

- `POST /v1/wpb/attestation` continued to return HTTP 200.
- A new verifier-valid Wallet Instance Attestation (WIA) could be issued.
- A previously issued WIA remained **VALID** on the status list.

Database verification in the controlled test environment showed that the MDVM device account no longer existed while the associated WPB account remained present and unrevoked.

For comparison, invoking the normal WPB revocation operation changed the WIA status to INVALID. This demonstrated that deletion and revocation were separate lifecycle operations.

## PNS Remains Active

PNS registration also continued to return a successful response after MDVM deletion.

This further demonstrated that deletion of the primary MDVM account did not cascade the lifecycle state to associated wallet services.

## Root Cause

The observed behavior indicated incomplete cross-service lifecycle coordination.

The MDVM token validation checked cryptographic properties but did not verify that the corresponding MDVM account still existed or had not been deleted/revoked.

The deletion handler removed the device account without cascading the lifecycle change to the associated WPB, RWSCA, and PNS records.

The RWSCA authorization relationship was based on the authentication-key thumbprint rather than the lifecycle state of the original MDVM account. Consequently, possession of the same authentication key remained sufficient to access the surviving RWSCA account.

WPB state and previously issued status-list entries were similarly not invalidated by MDVM deletion.

## Security Impact

The intended security boundary should be:

```text
MDVM wallet deleted/decommissioned
        ↓
Associated wallet capabilities revoked
```

The observed behavior was:

```text
MDVM wallet deleted
        ↓
WPB / RWSCA / PNS capabilities remain active
```

This means a deleted wallet can continue to:

- Perform RWSCA PIN and session operations.
- Create signing keys.
- Generate fresh valid ECDSA signatures.
- Obtain new verifier-valid WIAs.
- Keep previously issued WIAs in VALID status.
- Retain PNS registration.

This is particularly relevant to lost/stolen-device and wallet-decommissioning scenarios, where deletion is expected to terminate the associated security capabilities.

## Scope and Testing Limitations

Testing was performed against a local CI-test deployment using controlled accounts.

The demonstrated impact was limited to the wallet holder's own legitimate credentials. I did not access another user's wallet, impersonate another identity, or demonstrate cross-user account takeover.

A live sandbox validation was not performed because the sandbox required the iOS application's mTLS client certificate.

No real-user data was accessed.

## Program Outcome

The program closed the report as **Duplicate (#4046235)**.

The program stated that both reports described the same incomplete cleanup vulnerability involving failure to cascade revocation from MDVM deletion to WPB, RWSCA, and PNS services.

The program specifically noted similarities involving:

- Accounts surviving deletion.
- Re-registration continuing to access surviving service accounts.
- Missing cross-service lifecycle coordination.

The program consolidated the issue under the earlier comprehensive report.

Although the report was a duplicate and received no separate credit, this portfolio records the **actual vulnerability independently identified and technically validated**, including the additional validation involving a newly issued MDVM token, fresh ECDSA signatures, and surviving WPB attestations.

### Responsible Disclosure

This public entry is based on controlled testing and is sanitized. No real-user data, victim wallet, private credentials, or unnecessary deployment-specific evidence is published.
