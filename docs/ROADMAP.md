# Roadmap

Honest, staged, and ordered by what earns trust fastest. AURA's moat is depth and safety — not
feature count — so the roadmap is built around that.

## ✅ Phase 0 — Prototype (done · hackathon winner)

The trust-first autonomy loop, proven end-to-end.

- [x] Multi-agent reasoning (LangGraph `diagnose → plan → validate`, Bedrock / Claude 3.5)
- [x] Graceful degradation to a deterministic simulated brain
- [x] Shadow Mode orchestration (canary → verify → promote / rollback)
- [x] Multi-cloud healer (AWS live; Azure / GCP simulated behind one interface)
- [x] Safety layer: PII redaction, RBAC + multi-tenancy, blocked-action veto
- [x] FinOps idle scan + budget-lock policy
- [x] Predictive Z-score risk scoring
- [x] OpenTelemetry + Prometheus observability
- [x] Audit event bus → RCA PDF
- [x] AWS CDK infra (Lambda · SNS · CloudWatch alarm · API Gateway)
- [x] React + Vite decision-desk dashboard

## 🔜 Phase 1 — Productionise the core (Arcanum.ai program)

Turn the proof into something a platform team could pilot.

- [ ] **Real Shadow Mode** — provider-native canary primitives and true traffic shadowing against
      live infrastructure, replacing the orchestration-layer simulation
- [ ] **Hardened auth** — JWT / OIDC, real tenant isolation, scoped IAM roles per action
- [ ] **Real Azure & GCP adapters** — promote the labelled simulations to live SDK calls
- [ ] **Bedrock-native** — first-class Bedrock agent wiring (beyond the OpenAI-compatible path)
- [ ] **Signed, content-addressed RCA evidence** — tamper-evident audit certificates

## 🧭 Phase 2 — Integrate with the stack teams already run

Autonomy is only useful where the incidents actually surface.

- [ ] Observability ingest: Datadog, CloudWatch, Prometheus, OpenTelemetry collectors
- [ ] Incident sources: PagerDuty, Opsgenie, Slack
- [ ] ChatOps: approvals, status, and RCA delivered in Slack / Teams
- [ ] Runbook learning — AURA proposes and refines runbooks from resolved incidents
- [ ] Measured **MTTR reduction** on real customer telemetry (the number that sells the product)

## 🚀 Phase 3 — From healing to preventing

- [ ] Predictive remediation — act on the Z-score / anomaly signal *before* the alarm fires
- [ ] Policy-as-code safety envelopes customers author themselves
- [ ] SOC 2 Type II, enterprise compliance posture
- [ ] Self-serve onboarding and usage-based pricing

## Guiding principle

Every phase ships with its honesty label intact: what is real vs. simulated, what is proven vs.
claimed. Trust is the product; we will not trade it for a flashier demo.
