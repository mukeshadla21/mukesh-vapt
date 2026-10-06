# Finding #19 — Authenticated doveadm HTTP API Crash via Improper uint32 Validation

## 🐞 Bug Found — Responsible Disclosure

**Program:** Dovecot / Open-Xchange  
**Area:** doveadm HTTP API  
**Vulnerability:** Improper Input Validation / Worker-Level Denial of Service  
**Affected Command:** `mailbox update`  
**Parameter:** `uidValidity`  
**Severity:** Informative — program classification

## What I Found

I identified that the doveadm HTTP API allowed an out-of-range `uidValidity` value to reach the internal unsigned 32-bit integer parameter handling code.

Values outside the expected `uint32` range can reach an internal assertion instead of being rejected as normal API input validation errors.

The assertion:

```text
assertion failed: (value >= 0 && value <= (4294967295U))
```

causes the affected doveadm worker to terminate with `SIGABRT`.

## Affected Component

```text
Product:    Dovecot
Component:  doveadm HTTP API
Endpoint:   POST /doveadm/v1
Command:    mailbox update
Parameter:  uidValidity
Expected:   uint32
```

Exact deployment details are intentionally omitted from this public write-up.

## Reproduction

A valid mailbox update request was first confirmed to work normally.

The same request was then submitted with an out-of-range `uidValidity` value such as `-1`.

The server-side result was:

```text
Panic: file doveadm-cmd-parse.c: line 98
(doveadm_cmd_param_uint32): assertion failed:
(value >= 0 && value <= (4294967295U))

Fatal: master: service(doveadm): child killed with signal 6 (core dumped)
```

The behavior was also reproduced using the upper out-of-range value `4294967296`.

## Boundary Testing

The tested values produced the following results:

| uidValidity | Result |
|---:|---|
| -1 | Worker terminates with SIGABRT |
| 0 | Request succeeds |
| 1 | Request succeeds |
| 4294967295 | Request succeeds |
| 4294967296 | Worker terminates with SIGABRT |

This confirmed that the crash occurred specifically when the supplied value was outside the expected unsigned 32-bit range.

## Security Impact

An authenticated user with credentials permitting access to the affected doveadm HTTP API can terminate the worker handling the request by supplying an invalid `uidValidity` value.

The demonstrated impact is:

- Remote worker termination through the HTTP API.
- `SIGABRT` process termination.
- Repeated worker crash/restart activity when malicious requests are repeated.

Testing showed that the service creates a new worker after the crash and that a subsequent valid request can succeed.

Therefore, I did **not** demonstrate:

- Persistent service-wide denial of service.
- Remote code execution.
- Memory corruption.
- Privilege escalation.
- Authentication bypass.
- Confidentiality or integrity impact.
- Host compromise.

## Authentication / Required Access

The issue is authenticated.

The attacker requires valid credentials that permit access to the doveadm HTTP API and the affected command.

No SSH, shell, filesystem, local OS privileges, source-code access, or direct host access were required.

## Root Cause

The observed root cause is that an out-of-range JSON value reaches the internal `uint32` parameter handling code.

Instead of returning a normal input-validation error, the invalid value reaches a process-terminating assertion.

The expected range is:

```text
0 <= uidValidity <= 4294967295
```

## Recommended Remediation

- Validate `uidValidity` at the HTTP API boundary before invoking internal parameter conversion.
- Reject values outside the unsigned 32-bit range with a normal API validation error.
- Apply consistent type and range validation across interfaces accepting the parameter.
- Avoid allowing attacker-controlled API input to trigger process-terminating assertions.
- Add regression tests for negative values, values above `UINT32_MAX`, and valid boundary values.

## Program Outcome

The report was reviewed by Open-Xchange and classified as **Informative**.

The program stated that the affected API requires administrative rights and that the service configuration uses `client_limit=1`, meaning the demonstrated crash effectively terminates the requester's own worker/session. The program indicated it would address the behavior as a regular bug.

The finding is retained in this portfolio because it was a real, reproducible security-relevant input-validation flaw discovered and responsibly reported.

### Responsible Disclosure

This public entry is sanitized. Credentials, private deployment details, and unnecessary production evidence are not published.
