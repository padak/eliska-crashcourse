# Keboola Context for PRD Agent

This reference file provides Keboola-specific context to help the PRD agent ask relevant questions and identify misalignments.

## 1. Customer Segments

### Enterprise
**Key Requirements:**
- SLA guarantees (99.9%+ uptime)
- Custom deployment options (private cloud, on-premise)
- SSO/SAML integration
- Comprehensive audit logs
- Dedicated support and CSM
- Compliance certifications (SOC2, GDPR, HIPAA)

**Signals:** SLA, custom deployment, audit log, dedicated support, compliance

### Multi-tenant
**Key Requirements:**
- Collaboration features (sharing, permissions)
- Team management and role-based access
- Workspace organization
- Usage visibility and cost allocation

**Signals:** Team collaboration, workspace sharing, permissions, cost allocation

### PAYG (Pay As You Go)
**Key Requirements:**
- Quick start and onboarding (< 15 minutes to first value)
- Low friction signup (credit card, no sales call)
- Self-service documentation and tutorials
- Transparent, predictable pricing

**Signals:** Quick start, self-service, low friction, trial conversion

---

## 2. User Personas

### Data Engineers
- Build and maintain data pipelines
- Prefer CLI and API for automation
- Care about: pipeline reliability, data freshness, cost efficiency

**Tools:** SQL, Python, CLI, Git, observability tools

### Developers
- Build custom integrations and components
- Extend platform capabilities via API/SDK
- Care about: API reliability, documentation, developer experience

**Tools:** REST APIs, SDKs, webhooks, testing tools

### Business Users
- Consume and analyze data
- Create reports and dashboards
- Care about: time to insight, self-service, familiar tools

**Tools:** Visual interfaces, Excel, Google Sheets, BI tools

---

## 3. Challenge Patterns & Anti-patterns

### Pattern 1: Persona Mismatch

| Signal | Challenge |
|--------|-----------|
| PM says "data engineers" but problem is reporting/dashboards | "This sounds like a business user problem, not data engineer. Who's actually waiting for this output?" |
| Feature for "developers" but no API/SDK/CLI | "You're targeting developers but I don't see any API. How will they integrate this?" |
| Vague "users" without specifying who | "Which users specifically? Data engineers, developers, or business users? Each has different needs." |

### Pattern 2: Segment Mismatch

| Signal | Challenge |
|--------|-----------|
| SLA, custom deployment, audit log → but target is PAYG | "You mention SLA and audit logs - those are Enterprise features. Is PAYG the right target?" |
| Quick start, self-service → but target is Enterprise | "This sounds like PAYG onboarding. Why are we targeting Enterprise with self-service?" |
| Team collaboration → without considering Multi-tenant | "Sharing and collaboration are key for Multi-tenant. Should this segment be in scope?" |

### Pattern 3: Unrealistic Metrics

| Signal | Challenge |
|--------|-----------|
| "Improve developer experience" | "How will we MEASURE this? What's the baseline? Which dashboard shows it?" |
| "Increase satisfaction" | "NPS? CSAT? Support tickets? Define a concrete metric." |
| Metric without baseline | "You want to improve X, but what's the CURRENT value? Without baseline, we can't measure success." |
| Unmeasurable metric | "We don't track this in Keboola telemetry. Plan: add instrumentation, or use proxy metric?" |

### Pattern 4: Hidden Dependencies

| Signal | Challenge |
|--------|-----------|
| Feature assumes another feature exists | "This assumes Y is already working. Is that true? When does Y ship?" |
| Integration without checking API exists | "You want to integrate with X - do they have an API? Is it documented? Stable?" |
| "Simple change" in core system | "This touches [storage/queue/auth]. Do you have buy-in from platform team?" |

### Pattern 5: V1 Scope Creep

| Signal | Challenge |
|--------|-----------|
| "And it would also be nice to have..." | "Stop - is this V1 or V2? V1 should solve ONE clear problem." |
| 5+ features in "MVP" | "I count #{n} features. What's the ONE thing without which this doesn't make sense?" |
| "Easy quick win" with complex implementation | "You say quick win, but implementation requires #{n} changes. Is it really quick?" |

---

## 4. Hypothesis Examples

Agent PROPOSES, PM validates:

| Situation | Agent Hypothesis |
|-----------|------------------|
| PM mentions "large customers" | "I assume Enterprise segment - SLA and custom support will be key. Agree?" |
| PM talks about pipelines and scheduling | "Sounds like a data engineer problem. Are they our primary target?" |
| PM wants "faster export" | "I guess baseline is around 30s and target is under 5s. What's the reality?" |

---

## 5. Gap-Filling Prompts

**User Impact missing:**
> "Which Keboola users are affected?
> - Data engineers (pipelines, transformations, data quality)
> - Developers (API, SDK, integrations)
> - Business users (analysis, reports, dashboards)
>
> For each: What problem does this solve? How often?"

**Metrics are vague:**
> "Metric 'improve performance' isn't measurable:
> - CURRENT value? (e.g., job duration 45s)
> - TARGET value? (e.g., < 15s)
> - How to measure? (dashboard link)"

**Baseline missing:**
> "We don't have a baseline. Options:
> 1. Find dashboard in Keboola telemetry
> 2. Add instrumentation as V0 task
> 3. Proxy metrics (support tickets, feedback)"

---

## 6. Red Flags to Watch For

- Vague language: "users", "improve", "better", "easy", "simple"
- Metrics without baselines or targets
- Features targeting everyone
- "And also..." additions to scope
- Assumed infrastructure or capabilities
- Mismatch between problem and solution
- No mention of alternatives considered
