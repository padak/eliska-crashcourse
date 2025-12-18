# PRD: [Name of the project]

> **Template Purpose**: This template helps Keboola PMs create structured, metric-driven PRDs that align problem definition with solution approach.

---

## PROBLEM ALIGNMENT

### 1. PROBLEM DEFINITION

[Describe the core problem or opportunity here]

**User Impact**: [How does this affect data engineers, developers, or business users?]
- Example: Data engineers spend 30+ minutes debugging job failures due to unclear error messages.

**Business Impact**: [Impact on Enterprise retention, Multi-tenant growth, or PAYG expansion]
- Example: Enterprise customers cite poor error handling as a friction point in renewal discussions.

**Strategic Alignment**: [How this supports platform strategy - performance, DX, feature completeness]
- Example: Aligns with our Q1 2026 goal to improve developer experience.

**What we're NOT solving**: [Explicitly state out-of-scope problems]
- Example: We're not building a full debugging suite or automated fix suggestions in V1.

---

### 2. HIGH-LEVEL APPROACH

[Describe your approach to solving the problem]

**Scope indication**: [Give a rough sense of effort and boundaries]
- Example: This is a platform-wide initiative (L-sized). V1 focuses on Storage and Transformations.

---

### 3. SUCCESS METRICS

**For Enterprise Customers** (Platform Adoption & Retention):

- **Drive Adoption**: [Does it increase platform usage depth?]
  - Track: [Active flows, transformations run, data apps deployed]
  - Target: [e.g., "Increase monthly active transformations by 15%"]

- **Improve Developer Experience**: [Does it reduce friction?]
  - Track: [Time to first value, error rates, support tickets]
  - Target: [e.g., "Reduce error-related support tickets by 25%"]

- **Enhance Retention**: [Does it create stickiness?]
  - Track: [Monthly active projects, DAU/MAU ratio, churn indicators]
  - Target: [e.g., "Improve NPS by 5 points among data engineers"]

**For Multi-Tenant / PAYG Growth**:

- **Registration & Activation**: [Does it improve onboarding?]
  - Track: [Sign-up rate, time to first job, trial-to-paid conversion]
  - Target: [e.g., "Increase trial-to-paid conversion by 10%"]

- **Engagement**: [Does it drive continued usage?]
  - Track: [D7/D14/D30 retention, weekly active users]
  - Target: [e.g., "Improve D30 retention from 40% to 50%"]

- **Monetization**: [Does it drive PAYG revenue?]
  - Track: [PAYG revenue, upgrade rates, LTV]
  - Target: [e.g., "Increase average PAYG revenue per user by 20%"]

**For Platform Performance**:
- Track: [Job success rates, error rates, API response times]
- Target: [e.g., "Improve job success rate from 92% to 97%"]

**Trade-offs to consider**: [Could this harm other metrics?]

**Decision Checkpoint**: If we can't measure it, we shouldn't be doing this.

---

### 4. SET THE BASELINE

**Current Metrics** (as of [date range]):

- [Metric 1]: [Current value]
  - Trend: [Stable/Improving/Declining]

- [Metric 2]: [Current value]
  - Trend: [Stable/Improving/Declining]

**Dashboard Link**: [Link to analytics dashboard]

**If baseline unavailable**: [Explain why and provide alternatives]

**Decision Checkpoint**: We shouldn't proceed if we can't measure baseline.

---

## SOLUTION ALIGNMENT

### 5. KEY FEATURES

**Overview**: [High-level description of the solution]

**What we're NOT building**:
- [List explicitly excluded features]

---

**V1: Foundation / Quick Wins**

**Goal**: [Prove value hypothesis or resolve immediate pain points]

**Features**:
- [Feature 1]: [Brief description]
  - Success criteria: [Measurable outcome]

- [Feature 2]: [Brief description]
  - Success criteria: [Measurable outcome]

**Decision Checkpoint**: After V1 launch, evaluate:
- Did we hit target metrics?
- Should we continue to V2 or pivot?

---

**V2: Feature Completeness** (Optional)

**Goal**: [Make solution production-ready]

**Features**:
- [Feature 1]: [Brief description]
- [Feature 2]: [Brief description]

**Decision Checkpoint**: Has V1 proven valuable enough to justify V2?

---

**V3: Differentiation** (Optional)

**Goal**: [Create competitive advantage]

**Features**:
- [Feature 1]: [Brief description]
- [Feature 2]: [Brief description]

**Decision Checkpoint**: Does this create measurable competitive advantage?

---

## Additional Sections (Optional)

### TECHNICAL ARCHITECTURE
[High-level technical approach, major dependencies]

### DEPENDENCIES & RISKS
[Technical dependencies, known risks and mitigations]

### OPEN QUESTIONS
[Unresolved decisions, items needing stakeholder input]

---

**Template Version**: 1.0 (Dec 2025)
