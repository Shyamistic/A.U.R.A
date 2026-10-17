# Vision

> **Reliability without humans. Safety without compromise.**

## The thesis in one sentence

The bottleneck in autonomous operations is not intelligence — it's **trust**, and trust is earned by
proving a fix is safe *before* it ships, not by apologising after.

## Why now

Three curves are crossing at once:

1. **Systems got too complex for humans to hold in their heads.** Microservices, multi-cloud, and
   ephemeral infrastructure mean the person paged at 3 AM often has never seen the failing component.
2. **Models got good enough to reason about telemetry.** Frontier LLMs can read logs, correlate
   metrics, and draft a credible remediation — the diagnosis problem is largely solved.
3. **The action problem is still open.** Letting a model *execute* against production is where every
   serious buyer freezes. The category is stuck at "the AI suggests, a human approves."

AURA is built for the moment that gap closes — and it closes by making the first autonomous action
provably safe.

## What we believe

- **Autonomy must be earned, not assumed.** An agent that cannot prove a fix works in a sandbox has
  no business running it in production.
- **Safety is a system property, not a prompt.** PII redaction, RBAC, blocked-action governance, and
  cost guardrails are deterministic layers that exist whether or not the model behaves. The model
  proposes; the guardrails dispose.
- **Trust is built in the open.** We label what is real vs. simulated, we publish our limits, and we
  generate auditable evidence (RCA reports) for every decision. A black box that "just trust me"
  heals your infra is a non-starter for the enterprise.
- **The teammate, not the tool.** The goal is not a better alerting dashboard. It's an SRE teammate
  that takes the incident from *alerting* to *solved* while you sleep — and hands you a clean
  write-up in the morning.

## The long game

| Horizon | What AURA becomes |
|---|---|
| **Now** | A trust-first autonomous remediation loop with Shadow Mode, proven on AWS. |
| **Next** | A multi-cloud SRE platform that integrates with the observability stacks teams already run (Datadog, CloudWatch, Prometheus, PagerDuty) and reduces real MTTR on real telemetry. |
| **Later** | The autonomous operations layer for production software — not just healing incidents, but preventing them, with every action backed by sandbox-verified evidence. |

## What we are *not* building

- Not another read-only "investigate and recommend" copilot — the market is saturated with those.
- Not a chatbot bolted onto a dashboard.
- Not a system that asks you to trust it blindly. Every claim of autonomy is backed by a sandbox
  proof and an audit trail.

The wedge is **safe autonomous action**. Everything else is a feature.
