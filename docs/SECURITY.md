# Security & Trust

AURA's whole premise is acting autonomously in production. That is only acceptable if safety is a
**system property** — deterministic layers that hold regardless of what the model does. This document
describes how AURA earns the right to act.

> Reporting a vulnerability? See [the bottom of this page](#reporting-a-vulnerability).

## Defense in depth

```
request ─▶ RBAC + tenant check ─▶ rate limit ─▶ PII redaction ─▶ reasoning ─▶ blocked-action veto ─▶ Shadow Mode ─▶ heal ─▶ audit
```

Each layer is independent. A failure or misbehaviour in one does not disable the others.

### 1. The model never sees secrets

A **PII redactor** scrubs emails, IPv4 addresses, and AWS access keys from every payload **before**
it is logged or sent to the LLM. Redaction happens on the event bus, so audit logs and the model
prompt are both clean by construction.

### 2. Least privilege, by role and by tenant

Role-based access control with a `viewer / operator / admin / system_agent` matrix. The AI runs under
the narrowest `system_agent` scope. Every protected route requires an `X-Tenant-ID`; rate limiting is
keyed per tenant so one tenant cannot starve another.

### 3. The AI cannot do catastrophic things — ever

A **blocked-action veto** list is enforced on the agent's execution path. Destructive operations such
as `DeleteDBInstance`, `TerminateInstances`, and `DeleteBucket` are refused no matter how confident
the plan is. This is a hard stop, not a prompt suggestion.

### 4. Prove before you touch production

**Shadow Mode** rehearses every fix in an isolated canary and promotes only a change that stays
healthy. Production is never the test environment. See
[architecture/SHADOW_MODE.md](architecture/SHADOW_MODE.md).

### 5. Spend guardrails

The FinOps layer makes remediation cost-aware and can engage a spending freeze before an expensive
auto-scale — autonomy should not be able to run up a cloud bill.

### 6. Everything is audited

An append-only event bus records every decision, which can be exported as a per-incident **RCA PDF**.
Autonomy without an audit trail is unaccountable; AURA treats evidence as a first-class output.

## Transport & app hardening

- `helmet` security headers (frameguard deny, no-sniff, XSS filter, no-referrer)
- Request-body size limits and input validation (Zod)
- Per-request IDs and structured JSON logging for traceability
- CORS allow-listing (permissive in demo mode, strict in production configuration)

## Honest security status

This is a **prototype**. Auth is currently header/role based; **JWT / OIDC and real cryptographic
tenant isolation are the headline hardening items** on the [roadmap](ROADMAP.md). We state this
plainly rather than imply a production-grade posture we have not yet earned — which is the same
trust-first principle the product itself is built on.

## Reporting a vulnerability

Please do **not** open a public issue for security problems. Email the maintainers (see
[CONTACT.md](CONTACT.md)) with details and we will respond. We appreciate responsible disclosure.
