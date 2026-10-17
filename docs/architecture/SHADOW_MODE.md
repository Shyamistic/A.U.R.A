# Shadow Mode — the deep dive

Shadow Mode is AURA's core innovation and its go-to-market wedge. This document explains what it is,
why it matters, and how it works.

## The problem it solves

Autonomous remediation has a chicken-and-egg problem: you can't get the MTTR benefit of letting an
agent act until you trust it to act, and you can't build trust without letting it act. Every serious
buyer resolves this by keeping a human in the loop — which throws away most of the speed.

Direct-apply-with-rollback doesn't solve it either: by the time you roll back, production has already
seen the bad change. The blast radius already happened.

## The insight

**Move the proof before the blast radius.** Instead of applying the fix and watching production,
rehearse the fix somewhere isolated, watch *that*, and only promote what demonstrably works.

```
         ┌─────────────────────────────────────────────────────────┐
         │                     SHADOW MODE                          │
         │                                                          │
AI plan ─┼─▶ spin up isolated canary ─▶ route 5% shadow traffic ──┐ │
         │                                                        │ │
         │   ┌────────────── health probe ─────────────────┐     │ │
         │   │  latency ok?   error-rate ok?   stable?      │◀────┘ │
         │   └──────┬───────────────────────────┬──────────┘       │
         │          │ yes                        │ no               │
         │   promote to production        roll back + re-plan       │
         └──────────┼───────────────────────────┼──────────────────┘
                    ▼                            ▼
             ☁️ real environment           🔁 agent tries again
```

## How it works, step by step

1. **Propose** — the reasoning agent emits a structured remediation plan.
2. **Isolate** — AURA spins up a temporary canary fleet that receives a small slice of shadow traffic
   and is fully isolated from the live path.
3. **Rehearse** — the remediation is applied *to the canary only*.
4. **Verify** — a supervisor step watches the canary's health signals (latency, error rate,
   stability) against the known-good baseline.
5. **Decide**
   - **Healthy** → promote the exact same change to production.
   - **Degraded** → roll back the canary (production was never touched) and hand the failure back to
     the agent to re-plan a different approach.
6. **Record** — the outcome, the health deltas, and the promote/rollback decision all land in the
   audit log.

## Why this is the right wedge

- **It targets the market's #1 objection directly.** Buyers don't distrust AI diagnosis — they
  distrust AI *action*. Shadow Mode is a concrete, inspectable answer to "how do I know it won't make
  it worse?"
- **It's a feature competitors structurally lack.** Most AI-SRE tools are read-only by design
  precisely to avoid this problem. Offering safe action is a differentiated position, not a
  me-too feature.
- **It compounds with the rest of the safety layer.** Shadow Mode proves the fix *works*; RBAC,
  PII redaction, and blocked-action governance prove the fix is *allowed* and *clean*. Together they
  make autonomy defensible in front of a security review.

## Honest status

In the current prototype, Shadow Mode is implemented at the **orchestration layer**: the canary
spin-up, health check, and promote/rollback decision flow run end-to-end and are visible in the event
stream and dashboard. The production-grade version — real traffic shadowing against live
infrastructure with provider-native canary primitives — is the headline item on the
[roadmap](../ROADMAP.md). We label this distinction on purpose; overclaiming it would defeat the
entire trust-first premise of the product.
