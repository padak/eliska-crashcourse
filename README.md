# PRD Workflow Kit

PRD coach pro Keboola Product Managery. Agent "Elis" provází od nápadu přes research až po Linear submission.

## Co to umí

- **Hypothesis-driven přístup** - agent navrhuje hypotézy, PM validuje
- **Challenge patterns** - aktivně zpochybňuje předpoklady (persona mismatch, vague metriky)
- **Flexibilní vstup** - začni kde chceš (nápad, draft, review, update)
- **Linear integration** - automaticky vytvoří Project + Issue s PRD

## Workflow

```mermaid
flowchart TD
    Start([PM aktivuje agenta]) --> Entry{S čím přicházíš?}

    Entry -->|Nápad/Idea| P1[Phase 1: Discovery]
    Entry -->|Mám research| P3[Phase 3: Drafting]
    Entry -->|Mám draft PRD| P4[Phase 4: Review]
    Entry -->|Update existujícího| Update[Update Flow]

    P1 --> |Hypotézy + validace| P2[Phase 2: Challenge]
    P2 --> |Stress-test assumptions| P3
    P3 --> |Draft PRD| P4
    P4 --> |Quality check| Decision{Co dál?}

    Decision -->|Save locally| Save[.md soubor]
    Decision -->|Submit| P5[Phase 5: Linear]
    Decision -->|Pause| Pause([Vrátím se později])

    P5 --> |Create Project + Issue| Done([PRD v Linear])

    Update --> |Find in Linear| LoadPRD[Load existing PRD]
    LoadPRD --> |PM feedback| P4

    style Start fill:#e1f5fe
    style Done fill:#c8e6c9
    style Save fill:#c8e6c9
    style Pause fill:#fff3e0
```

## Jak začít

Řekni agentovi jedno z:
- "Elis"
- "potřebuju PRD"
- "nový PRD"
- "feature idea"

Agent se zeptá, s čím přicházíš, a navede tě správnou cestou.

## Challenge Patterns

Agent aktivně hledá slabá místa:

| Pattern | Příklad |
|---------|---------|
| **Persona mismatch** | "Říkáš data engineers, ale tohle zní jako business user problém..." |
| **Segment mismatch** | "SLA requirements naznačují Enterprise, ne PAYG..." |
| **Vague metrics** | "'Improve performance' není měřitelné - jaká je baseline?" |
| **Hidden dependencies** | "Tohle předpokládá, že X už funguje. Je to pravda?" |

## Struktura

```
prd-workflow/
├── agents/
│   └── keboola-prd-coach.md    # Agent Elis
└── skills/
    └── prd-linear/
        ├── SKILL.md             # Linear submission logic
        └── references/
            ├── prd-template.md      # PRD šablona
            └── keboola-context.md   # Keboola kontext
```

## Requirements

- Claude Code CLI
- Linear MCP server (pro submission):
  ```bash
  claude mcp add --transport sse linear-server https://mcp.linear.app/sse
  ```

## License

MIT
