# Finding #16 — Unbounded Request Body in cardano-submit-api Enables Memory Exhaustion DoS

## 🐞 Bug Found — Responsible Disclosure

**Program:** Intersect / Cardano Security  
**Area:** API / Denial of Service  
**Affected Component:** `cardano-submit-api`  
**Vulnerability:** Unbounded HTTP Request Body / Memory Exhaustion DoS  
**Severity:** High  
**CVSS v3.1:** 7.5 — `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H`

## What I Found

During source-code and security testing of the Cardano `cardano-submit-api`, I identified a request-body handling condition that can allow an unauthenticated remote attacker to cause excessive memory consumption.

The transaction submission API accepts the request body as a CBOR stream. The corresponding MIME unmarshaller converts the complete lazy request body into a strict `ByteString`:

```haskell
instance MimeUnrender CBORStream ByteString where
    mimeUnrender _ = Right . LBS.toStrict
```

This means the complete attacker-controlled request body can be materialized in memory before transaction processing occurs.

The reviewed HTTP server configuration did not enforce an effective maximum request-body size before this conversion.

## Technical Details

The relevant request flow was identified around:

- `cardano-submit-api/src/Cardano/TxSubmit/Rest/Types.hs`
- `cardano-submit-api/src/Cardano/TxSubmit/Types.hs`
- `cardano-submit-api/src/Cardano/TxSubmit/Web.hs`

The transaction submission path subsequently passes the materialized bytes into transaction deserialization.

The affected API is the transaction submission endpoint:

```text
POST /api/tx/submit
```

Exact production host information is intentionally omitted from this public write-up.

## Proof of Concept

Testing was performed only against a researcher-controlled/test instance.

A controlled oversized request body was sent to the transaction submission endpoint to observe memory consumption. The test demonstrated substantial memory usage by the `cardano-submit-api` process.

No production Cardano infrastructure was targeted.

## Security Impact

An attacker able to reach the affected submit-api service could potentially:

- Consume excessive process memory.
- Cause the `cardano-submit-api` service to become unavailable.
- Trigger process termination through operating-system OOM handling.
- Prevent transaction submissions through the affected instance.

The demonstrated security impact is a **denial of service against the submit-api service**.

I did not demonstrate compromise of the underlying Cardano node, consensus layer, transaction integrity, or confidentiality.

## Recommended Remediation

Implement an explicit maximum request-body size at the HTTP/application layer and reject oversized requests **before** the complete body is converted into a strict `ByteString`.

Additional recommendations:

1. Configure appropriate request timeouts.
2. Apply request-size limits consistently to transaction submission endpoints.
3. Add deployment-level request-size restrictions at reverse proxies or load balancers where applicable.
4. Add regression tests confirming oversized request bodies are rejected without excessive memory consumption.
5. Review the request parsing path to avoid unnecessary full-body materialization for untrusted input.

A body-flushing or connection-management setting should not be treated as a substitute for an explicit maximum total request-body size.

## Responsible Disclosure

The vulnerability was reported privately to the Intersect security team.

Testing was limited to a researcher-controlled/test environment. No production infrastructure was subjected to the proof-of-concept request, and no user or transaction data was accessed.

This public portfolio entry intentionally omits private target details, production infrastructure information, and complete exploit instructions.
