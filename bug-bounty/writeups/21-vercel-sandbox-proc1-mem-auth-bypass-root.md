# Finding #21 — Writable /proc/1/mem Enables sandbox-init Authentication Bypass and Root Escalation

## 🐞 Bug Found — Responsible Disclosure

**Program:** Vercel Sandbox  
**Report:** #3981783  
**Area:** Sandbox Isolation / Privilege Boundary  
**Vulnerability:** Writable `/proc/1/mem` → Privileged Process Modification → Authentication Bypass → Root Escalation  
**Severity Assessed:** High — CVSS 4.0: 8.5  
**Program Outcome:** Not Applicable / Out of Scope

## What I Found

I identified a privilege-boundary issue inside a Vercel Sandbox where an unprivileged process running as the normal `vercel-sandbox` user could access the memory of the privileged `sandbox-init` process through writable `/proc/1/mem`.

The sandbox-init process exposes an internal authenticated gRPC service. By modifying the privileged process memory, I demonstrated that the authentication verification path could be altered and that an attacker-controlled request could reach the internal process-spawning functionality.

The resulting spawned process retained `CAP_SETUID`, allowing escalation from the normal sandbox user to `uid=0(root)` inside the Firecracker guest.

## Demonstrated Attack Chain

The reproduced chain was:

```text
Unprivileged Sandbox process
        ↓
Writable /proc/1/mem
        ↓
Modification of privileged sandbox-init
        ↓
Internal gRPC authentication bypass
        ↓
Process spawning
        ↓
CAP_SETUID
        ↓
uid=0(root) inside Firecracker guest
```

The reproduction was performed within a researcher-controlled Vercel Sandbox.

## Impact

The demonstrated impact included:

- Modification of the privileged `sandbox-init` process.
- Bypass of the internal gRPC authentication mechanism.
- Arbitrary command execution through the internal process-spawning service.
- Escalation from `uid=1000(vercel-sandbox)` to `uid=0(root)` inside the Firecracker guest.
- Additional Linux capabilities were observed during testing.

I did **not** demonstrate:

- EC2 host compromise.
- Firecracker host escape.
- Cross-tenant access.
- Access to another customer's Sandbox.
- Defeat of an external firewall or credential-brokering boundary.

## Program Validation and Outcome

Vercel Sandbox reviewed the report and stated that they **verified the reported chain on a live sandbox**, including writable `/proc/1/mem`, modification of the sandbox-init authentication path, command execution through Spawn, and `uid=0` inside the Firecracker guest.

The report was nevertheless closed as **Not Applicable** because the program's policy explicitly considered this class of container-to-Firecracker-guest-OS escape and guest-level root escalation to be an already-known/out-of-scope impact category.

The program indicated that an extension to a materially new impact, such as EC2 host compromise, cross-tenant access, or a qualifying firewall/credential-brokering bypass, could be considered separately.

## Research Significance

Although the report was classified as out of scope, it demonstrated a complete and reproducible privilege-escalation chain across an intended sandbox boundary and was independently validated by the program.

The finding is therefore retained in this portfolio as a security research contribution with its actual program outcome clearly documented.

### Responsible Disclosure

This public entry intentionally omits the working PoC, memory offsets, authentication-patching details, credentials, and other exploit-specific information. No other customer's Sandbox, data, credentials, or infrastructure was accessed.
