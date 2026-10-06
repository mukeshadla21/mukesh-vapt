# Finding #18 — Authenticated FILTER SIEVE SCRIPT Disconnect Can Crash a Dovecot Worker

## 🐞 Bug Found — Responsible Disclosure

**Program:** Dovecot / Open-Xchange  
**Area:** IMAP / Pigeonhole  
**Affected Version:** Dovecot 2.4.5 with Pigeonhole 2.4.5  
**Vulnerability:** Worker-Level Denial of Service / Crash  
**Severity:** Informational — program classification  
**Status:** Informative / Closed

## What I Found

I identified a remotely triggerable crash in the IMAP `FILTER SIEVE SCRIPT` command handling path.

An authenticated IMAP user can start a FILTER command, provide a Sieve script literal, intentionally leave the command waiting for its remaining search criteria, and then disconnect.

During client disconnect, Dovecot attempts to cancel the outstanding FILTER command. Instead of cancelling cleanly, the command reaches an internal panic:

```text
Panic: command didn't cancel itself: FILTER
```

The affected IMAP worker is then terminated with `SIGABRT`.

## Reproduction

The issue can be reproduced using a normal authenticated IMAP account:

1. Authenticate to IMAP.
2. Select `INBOX`.
3. Send a `FILTER SIEVE SCRIPT` command containing a small Sieve script.
4. Do not provide the remaining search criteria required to complete the command.
5. Disconnect the client while FILTER is still active.
6. Observe the server-side IMAP worker terminate.

A sanitized protocol sequence is:

```text
a1 LOGIN <authorized-test-user>
a2 SELECT INBOX
a3 FILTER SIEVE SCRIPT {<script-length>}
<test Sieve script>
<disconnect>
```

Testing was performed against a controlled deployment.

## Server-Side Evidence

A clean reproduction produced the following server-side conditions:

```text
Panic: command didn't cancel itself: FILTER
Fatal: master: service(imap): child killed with signal 6 (core dumped)
```

This confirms that the IMAP worker handling the triggering connection was terminated.

## Impact Validation

I performed a three-session test to determine whether the crash affected other IMAP users or the complete service.

Observed behavior:

- **Triggering session:** FILTER command caused the worker crash and connection termination.
- **Existing session belonging to another user:** continued to respond successfully to NOOP, LIST, and LOGOUT.
- **New IMAP connection after the crash:** established successfully and responded to NOOP.

Therefore, the demonstrated impact is a **worker-level crash**, not a demonstrated persistent service-wide denial of service.

## Security Impact

The attack requires a valid IMAP account, but does not require:

- SSH access.
- Shell access.
- Local filesystem access.
- Local operating-system privileges.
- Source-code access.
- Direct access to the Dovecot host.

The demonstrated impact is limited to terminating the IMAP worker handling the triggering connection.

I did not demonstrate:

- Cross-user impact.
- Persistent service-wide outage.
- Code execution.
- Memory corruption.
- Data disclosure.

## Root Cause

The observed root cause is an incomplete cancellation path for the FILTER command.

When the client disconnects while FILTER is waiting for additional input, the command does not cancel itself as expected and reaches the internal panic condition:

```text
command didn't cancel itself: FILTER
```

The panic results in `SIGABRT`, terminating the worker handling the affected connection.

## Recommended Remediation

The FILTER cancellation path should safely handle client disconnects while the command is waiting for additional input.

Recommended changes include:

- Cancel and clean up the FILTER command normally when the client connection closes.
- Ensure FILTER transitions to a cancelled/terminated state before the worker reaches a fatal assertion.
- Add regression tests covering client disconnects during incomplete FILTER commands.
- Avoid process-terminating panic/error handling for attacker-controlled protocol states where recovery is possible.

## Program Outcome

The report was reviewed by the program and classified as **Informative** because the demonstrated impact was limited to killing the attacker's own IMAP session/worker.

The finding is still documented here because it was a real, reproducible security-relevant behavior discovered and responsibly reported during testing.

### Responsible Disclosure

The public portfolio entry is sanitized. Test credentials, private infrastructure details, and unnecessary operational evidence are not published.
