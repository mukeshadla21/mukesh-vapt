# Finding #20 — Unauthenticated Dovecot Stats Service Crash via Malformed Chunked HTTP Request

## 🐞 Bug Found — Responsible Disclosure

**Program:** Dovecot / Open-Xchange  
**Area:** Stats / OpenMetrics HTTP Service  
**Vulnerability:** Unauthenticated Remote Crash / Denial of Service  
**Severity:** High potential impact — demonstrated worker-level crash  
**Affected Endpoint:** `GET /metrics`

## What I Found

I identified an unauthenticated crash condition in the Dovecot stats/OpenMetrics HTTP service.

A remote client can send a malformed HTTP request using `Transfer-Encoding: chunked` with an invalid chunk size. During request parsing and teardown, the stats OpenMetrics handler can reach an internal assertion in `http_server_response_request_free()`.

The assertion causes the stats child process to terminate with `SIGABRT`.

No authentication is required for the crash request.

## How It Worked

The tested sequence was:

```text
GET /metrics HTTP/1.1
Host: x
Transfer-Encoding: chunked

ZZZ
x
```

The invalid chunk size causes request processing to fail while an OpenMetrics response payload stream is active.

The observed server-side assertion was:

```text
stats: Panic: file http-server-response.c: line 84
(http_server_response_request_free): assertion failed:
(resp->payload_stream == NULL)
```

followed by:

```text
stats: Fatal: master: service(stats): child <PID>
killed with signal 6 (core dumped)
```

## Security Impact

The crash request is unauthenticated and can terminate the affected Dovecot stats/OpenMetrics worker with a single malformed request.

In the tested configuration, the Dovecot master automatically respawned the stats process. I did not demonstrate a persistent outage of the IMAP, authentication, or mail services.

However, the crash interrupts the stats service and can cause accumulated in-memory OpenMetrics telemetry to be lost when the worker is restarted.

## Telemetry Loss

I also tested whether restarting the stats process affected accumulated OpenMetrics state.

Before triggering the crash, the metrics endpoint contained non-zero counters including:

```text
dovecot_auth_successes_total
dovecot_mail_user_session_finished_total
```

After the unauthenticated malformed request caused the stats worker to terminate and the service respawned, the previously non-zero counters returned to zero in the tested deployment.

This demonstrates loss of accumulated in-memory OpenMetrics telemetry across the affected worker restart.

The IMAP login used during the telemetry test was only used to create baseline counter values. The request that triggered the crash contained no authentication credentials.

## Validation

The issue was reproduced using a controlled/test deployment.

The test demonstrated:

1. Stats/OpenMetrics service accessible remotely.
2. No authentication required for the crash request.
3. Malformed chunked request triggers the assertion.
4. Stats worker terminates with `SIGABRT`.
5. Dovecot respawns the stats worker.
6. Previously accumulated in-memory telemetry is reset in the tested configuration.

No high-rate or prolonged crash testing was performed against vendor infrastructure.

## Root Cause

The observed failure occurs during malformed chunked-request handling.

The OpenMetrics handler creates a response payload stream before the HTTP request body has been completely processed. When malformed chunk parsing causes request teardown, `http_server_response_request_free()` encounters an attached payload stream and reaches an assertion rather than safely cancelling/releasing the stream.

The resulting assertion terminates the stats worker.

## Recommended Remediation

- Safely handle response payload streams during malformed-request teardown.
- Ensure the payload stream is cancelled or released before final response cleanup.
- Do not allow malformed HTTP input to reach a process-terminating assertion.
- Return a normal HTTP error or safely close the connection when chunk parsing fails.
- Add regression tests covering malformed chunk sizes while an OpenMetrics response payload stream is active.
- Verify that worker restart behavior does not unintentionally discard required telemetry state.

## Responsible Disclosure

The issue was reported privately to the Dovecot security program.

This public entry is sanitized and omits private infrastructure details, credentials, and unnecessary operational evidence.
