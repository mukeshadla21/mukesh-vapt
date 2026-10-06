# Finding #22 — Unauthenticated OpenTelemetry Ingestion Allows Forged Telemetry Injection

## 🐞 Bug Found — Responsible Disclosure

**Program:** Crypto.com  
**Report:** #3932139  
**Area:** Observability / API Security  
**Vulnerability:** Missing Authentication / Telemetry Integrity  
**Affected Service:** `exchange-fe.crypto.com`  
**Program Outcome:** Duplicate (#3592648)

## What I Found

I identified publicly accessible OpenTelemetry (OTel) ingestion endpoints that accepted attacker-controlled logs, metrics, and traces without authentication.

The affected endpoints were:

```text
POST /public/otel/v1/logs
POST /public/otel/v1/metrics
POST /public/otel/v1/traces
```

Unauthenticated requests to the endpoints returned **HTTP 201**, indicating that the telemetry payloads were accepted.

The log endpoint also allowed attacker-controlled telemetry attributes, including `service.name` and log body content.

## Proof of Concept

A sanitized example of the tested log submission was:

```json
{
  "resourceLogs": [
    {
      "resource": {
        "attributes": [
          {
            "key": "service.name",
            "value": {
              "stringValue": "exchange-be"
            }
          }
        ]
      },
      "scopeLogs": [
        {
          "scope": {},
          "logRecords": [
            {
              "severityText": "ERROR",
              "body": {
                "stringValue": "MANUAL-VERIFY-PROOF"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

No authentication cookie, API key, bearer token, or other credential was supplied.

The server accepted the request with:

```text
HTTP 201
```

The logs, metrics, and traces endpoints were all tested without authentication and returned HTTP 201.

## Application Processing Validation

I also submitted malformed input to determine whether the request was reaching application-side processing.

The application returned an error indicating that it attempted to process the supplied telemetry structure rather than simply accepting the request at a static frontend layer.

The observed response was an HTTP 500 application error related to the malformed telemetry object.

## Security Impact

The primary demonstrated impact was **telemetry integrity**.

An unauthenticated attacker could potentially submit forged telemetry that appears to originate from an internal service identity such as `exchange-be`.

Depending on how the telemetry is consumed, attacker-controlled records could potentially:

- Create false alerts or misleading operational events.
- Spoof internal service identities.
- Dilute genuine security or operational signals.
- Complicate incident investigation.
- Reduce the reliability of monitoring and troubleshooting data.

I did **not** demonstrate:

- Customer account compromise.
- Customer data access.
- Customer fund access.
- Stored XSS.
- Dashboard compromise.
- Access to internal systems.

Testing was limited to verifying unauthenticated telemetry submission and application processing.

## Recommended Remediation

- Require authentication for OTel ingestion endpoints.
- Restrict ingestion to trusted telemetry producers or an authenticated OpenTelemetry Collector.
- Consider strong service-to-service authentication such as mTLS where appropriate.
- Validate telemetry resource attributes against approved service identities.
- Apply rate limits and reasonable payload/record-size limits.
- Avoid exposing unnecessary internal application exception details.
- Review existing telemetry for attacker-generated records and handle them appropriately.

## Program Outcome

Crypto.com reviewed the report and closed it as **Duplicate (#3592648)**, confirming that the issue had previously been reported.

Although the submission was a duplicate and did not receive separate credit, this portfolio records the **actual bug I independently found and responsibly reported**, together with the program's final disposition.

### Responsible Disclosure

This public entry is sanitized and excludes unnecessary operational details and private evidence. No customer accounts, customer data, or customer funds were accessed or modified during testing.
