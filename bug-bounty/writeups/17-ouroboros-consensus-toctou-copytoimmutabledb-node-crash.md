# Finding #17 — TOCTOU Race in ouroboros-consensus Can Trigger Node Crash

## 🐞 Bug Found — Responsible Disclosure

**Program:** Intersect / Cardano Security  
**Repository:** `IntersectMBO/ouroboros-consensus`  
**Area:** Consensus / ChainDB  
**Vulnerability:** Time-of-Check Time-of-Use (TOCTOU) Race Condition  
**CWE:** CWE-367  
**Severity:** High  
**CVSS v3.1:** 7.5 (provisional) — `CVSS:3.1/AV:N/AC:H/PR:L/UI:N/S:C/C:N/I:N/A:H`

## What I Found

During source-level security research of `ouroboros-consensus`, I identified a potential TOCTOU race in the ChainDB `copyToImmutableDB` path.

The operation reads the current chain state in one STM transaction, performs disk I/O, and later calls `removeFromChain` in a separate STM transaction.

During this interval, `switchTo` can replace the current chain.

If the later operation uses a stale block point that no longer belongs to the current chain, the code can reach a fatal error path:

```text
header to remove not on the current chain
```

## Relevant Code Paths

The source analysis identified the following areas as relevant to the race:

- `Background.hs` — initial STM read of `cdbChain`
- `Background.hs` — VolatileDB / ImmutableDB disk operations
- `Background.hs` — subsequent `removeFromChain`
- `Background.hs` — fatal error handling
- `ChainSel.hs` — `switchTo` updates `cdbChain`
- `Background.hs` — linked-thread exception propagation

Exact line references may vary between repository revisions.

## Race Condition

The suspected ordering is:

```text
Thread A: copyToImmutableDB reads current chain
        ↓
Thread A: performs disk I/O
        ↓
Thread B: switchTo replaces cdbChain
        ↓
Thread A: removeFromChain uses stale block point
        ↓
Stale point no longer matches current chain
        ↓
Fatal error path
```

The security concern is that chain state validation and subsequent mutation are separated by operations that allow the relevant state to change.

## Proof of Concept

I created a deterministic two-phase model representing the relevant concurrency ordering:

1. Thread A reads the current chain.
2. Thread B replaces the chain.
3. Thread A performs the equivalent of `removeFromChain`.
4. The previously observed block point no longer matches the current chain.
5. The fatal condition is triggered.

The model reproduced the condition in **20/20 iterations**.

This model was used to demonstrate the TOCTOU ordering and was not run against Cardano production infrastructure.

## Security Impact

If the race is reachable in the production implementation, an affected node could terminate while processing a competing fork.

Potential impact includes:

- Node denial of service.
- Temporary loss of node availability.
- Missed blocks while the node recovers or restarts.
- Broader availability impact if multiple nodes are independently exposed to the same condition.

The attack requires meaningful block-production capability/stake and favorable timing, so the reported severity was assessed as **High rather than Critical**.

No consensus compromise, transaction manipulation, or confidentiality impact was demonstrated.

## Recommended Remediation

Consider eliminating the stale-state race by making chain validation and the corresponding mutation atomic, or otherwise ensuring that `removeFromChain` cannot act on a stale chain point.

Additional hardening should include:

- Revalidate the chain state immediately before destructive/mutating operations.
- Introduce appropriate synchronization between `switchTo` and `copyToImmutableDB`.
- Replace fatal `error` paths with explicit recoverable error handling where appropriate.
- Add deterministic concurrency/regression tests that exercise chain switching during the copy-to-immutable workflow.

## Responsible Disclosure

The vulnerability was reported privately to the Intersect security team.

No production Cardano infrastructure was targeted with the proof of concept. This public portfolio entry intentionally omits private evidence and operational details that could facilitate attacks against live infrastructure.
