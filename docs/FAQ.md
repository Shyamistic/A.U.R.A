# FAQ

Straight answers, including to the uncomfortable questions.

### Is AURA a production product I can buy today?

No. AURA is an **early-stage prototype with a validated thesis** that won the AWS Global Vibe: AI
Coding Hackathon 2025 and is being carried forward through the Arcanum.ai productionisation program.
We label what is real vs. simulated everywhere, on purpose.

### What is actually real vs. simulated?

- **Real:** the multi-agent reasoning loop (LangGraph), the AWS healing adapter (live Lambda SDK calls
  with a simulation fallback), the FinOps EC2/CloudWatch scan, PII redaction, RBAC, the blocked-action
  veto, OpenTelemetry + Prometheus observability, the RCA PDF generation, and the CDK infrastructure.
- **Simulated / labelled:** the Azure and GCP healing adapters, some demo signals (e.g. the sampled
  idle check), and Shadow Mode at the orchestration layer (real traffic shadowing is the next step).

We publish this breakdown rather than blur it — see [ROADMAP.md](ROADMAP.md).

### How is this different from Resolve AI, Cleric, or Datadog Bits AI?

Those tools overwhelmingly do **read-only investigation and recommend a fix**, because trusting an
agent to *act* on production is the market's biggest objection. AURA's wedge is **safe autonomous
action** via **Shadow Mode** — prove the fix in an isolated canary, promote only on success. See
[MARKET.md](MARKET.md) and [architecture/SHADOW_MODE.md](architecture/SHADOW_MODE.md).

### Why should I trust an AI to touch my production infrastructure?

You shouldn't — blindly. That's the point. AURA earns trust with deterministic guardrails that hold
regardless of the model: the model never sees secrets (PII redaction), it runs least-privilege (RBAC),
it physically cannot run catastrophic operations (blocked-action veto), it proves every fix in a
sandbox first (Shadow Mode), and every decision is audited (RCA PDF). See [SECURITY.md](SECURITY.md).

### What happens if the LLM is unavailable?

AURA degrades gracefully. If no model or key is reachable, a deterministic `SimulatedBrain` returns a
valid plan so the system keeps functioning and the demo never hard-fails.

### Which clouds does it support?

AWS is live (real SDK calls). Azure and GCP are implemented behind the same adapter interface as
labelled simulations; promoting them to live SDK calls is a Phase-1 roadmap item.

### What did you build it with?

Amazon Bedrock (Claude 3.5 Sonnet) for reasoning, LangGraph for agent orchestration, Node.js/Express
for the backend, React/Vite for the dashboard, and AWS CDK for infrastructure. Development was
accelerated with Amazon Q Developer.

### Where is the source code?

The engine source lives in a **separate private repository**. This repository is AURA's public
documentation and brand home.

### Is "AURA" affiliated with any other project named AURA?

No. "AURA" here stands for our Autonomous SRE platform. It is unrelated to other tools or libraries
that happen to share the name.

### How can I get involved or follow along?

See [CONTACT.md](CONTACT.md).
