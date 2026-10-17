# Demo script

Watch AURA take an incident from alert to resolved — with every AI fix proven in a sandbox first.
~90 seconds.

> This repository is AURA's public documentation and brand home. The runnable engine lives in a
> separate private repository; this script describes the demo flow and the API surface so reviewers
> understand exactly what the product does.

## The 90-second story

1. **Open the dashboard.** The decision desk shows a live agent graph, a cloud map, a timeline, and
   a colored event log — all reading from the orchestrator.
2. **Give it a goal.** Hit the mic and say **"Deploy production stack"** (or `POST /goal`). AURA
   routes it through **Shadow Mode**: a canary spins up, health is verified, then the change is
   promoted. The timeline narrates each step.
3. **Inject chaos.** Click **🐢 Latency** (or `POST /simulate`). The Monitor node turns **red**.
4. **Watch the loop.**
   - The reasoning agent produces a **plan** (action, target, confidence) with a **cost estimate** and
     a **policy citation**.
   - **Shadow Mode** verifies the fix on an isolated canary.
   - The healer executes; the nodes turn **green** as the incident resolves.
5. **Pull the evidence.** Download the **RCA PDF** for the incident — a clean, auditable write-up.

## The API surface (what the demo exercises)

Every protected route requires tenant + role headers:

```
-H "Content-Type: application/json" -H "X-Tenant-ID: demo" -H "X-Role: operator"
```

| Endpoint | What it does |
|---|---|
| `POST /simulate` | inject chaos (`type`, `severity`, `provider`) → detect → diagnose → heal |
| `POST /goal` | run a goal through Shadow Mode (canary verify → promote) |
| `POST /optimize-costs?autoFix=true` | FinOps audit + optional auto-remediation |
| `GET /risk-score` | live predictive Z-score anomaly / risk gauge |
| `GET /events` | the audit event stream (RBAC: `viewer`+) |
| `GET /report/download/:id` | per-incident RCA PDF |
| `GET /metrics` | Prometheus metrics (incident counter, healing-duration histogram) |
| `GET /health` | liveness + version |

## Example calls

```bash
H='-H "Content-Type: application/json" -H "X-Tenant-ID: demo" -H "X-Role: operator"'

# chaos → heal loop
curl -X POST http://localhost:10000/simulate $H -d '{"type":"LATENCY","severity":"HIGH","provider":"aws"}'

# shadow-verified deploy
curl -X POST http://localhost:10000/goal $H -d '{"goal":"Deploy production stack"}'

# FinOps audit + auto-fix
curl -X POST "http://localhost:10000/optimize-costs?autoFix=true" $H

# live risk score
curl http://localhost:10000/risk-score
```

## What to look for

- The **plan is structured JSON** with a confidence score — not free text.
- The **Shadow Mode** step always runs before any heal — that's the whole point.
- The **blocked-action veto** refuses destructive operations even if you try to force them.
- The **event log is already PII-scrubbed** — no emails, IPs, or keys leak into the audit trail.
