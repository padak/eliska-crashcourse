# Plan: PRD Workflow Kit pro Keboola

## Cil

Vytvorit kompletni workflow pro Product Managery v Keboole:
1. **Agent** ktery provadi PM od napadu k hotovemu PRD (product research + drafting)
2. **Skill** ktery umoznuje submission do Linearu
3. **Onboarding** pro nove PM (instalace, Linear MCP setup)

---

## Architektura

```
Eliska-crashcourse/
├── prd-workflow/
│   ├── INSTALL.md                    # Onboarding instrukce pro PM
│   ├── agents/
│   │   └── keboola-prd-coach.md      # Hlavni agent - provazi PM celym workflow
│   └── skills/
│       └── prd-linear/
│           ├── SKILL.md              # Linear submission skill
│           └── references/
│               ├── prd-template.md   # Keboola PRD sablona
│               └── keboola-context.md # Segmenty, metriky, terminologie
├── scripts/
│   └── install-prd-workflow.sh       # Instalacni script
└── docs/
    └── michal_jerabek.md             # Zdroj PRD sablony (konvertovano z PDF)
```

### Proc tato architektura?

| Puvodni plan | Novy plan |
|--------------|-----------|
| Jeden monoliticky skill | Agent (mysli) + Skill (kona) |
| PM ridi workflow manualne | Agent provazi PM automaticky |
| Zacina validaci | Zacina product researchem |
| Skill v ~/.claude | Repo + install script |

---

## Komponenty

### 1. Agent: keboola-prd-coach.md

**Umisteni:** `prd-workflow/agents/keboola-prd-coach.md`

**Ucel:** Hlavni vstupni bod pro PM. Provazi celym procesem od napadu po Linear submission.

**Frontmatter:**
```yaml
---
name: keboola-prd-coach
description: PRD coach pro Keboola PM. Pouzij pri "novy PRD", "feature idea", "product spec", "napiste PRD". Provazi od napadu pres research az po Linear submission.
model: opus
---
```

**Faze workflow:**

```
Faze 1: Discovery & Clarification
├── Analyze PM's idea for gaps
├── Ask 3-5 clarifying questions (grouped by category)
├── Identify target segment (Enterprise / Multi-tenant / PAYG)
└── Confirm: "Ready to research?"

Faze 2: Challenge & Exploration
├── Identify implicit assumptions
├── Ask "what-if" questions (stress-test)
├── Research competitors / existing solutions (optional)
└── Confirm: "Proceed to drafting?"

Faze 3: PRD Drafting
├── Draft each section using prd-template.md
├── Validate against Keboola context (metrics, segments)
├── Interactive gap-filling (cilene otazky)
└── Confirm: "PRD ready for review?"

Faze 4: Review & Polish
├── Show complete PRD preview
├── Check quality criteria (konkretni metriky, ne vague)
├── PM approves or requests changes
└── Confirm: "Submit to Linear?"

Faze 5: Linear Submission
├── Use prd-linear skill
├── Select team, check duplicates
├── Create Project + Issue
└── Return URLs + next steps
```

**Klicove vlastnosti:**
- Zna Keboola kontext (Enterprise vs PAYG vs Multi-tenant)
- Challenge assumptions - neni "yes-man"
- Konkretni priklady z Keboola prostredi
- Automaticky vola skill pro Linear (PM nemusi vedet ze existuje)

---

### 2. Skill: prd-linear

**Umisteni:** `prd-workflow/skills/prd-linear/`

**Ucel:** Helper skill pro Linear operace. PM ho nevola primo - agent ho pouziva v Fazi 5.

**SKILL.md Frontmatter:**
```yaml
---
name: prd-linear
description: Linear submission pro Keboola PRD. Vytvori Project + Issue s PRD v description. Pouzivano agentem keboola-prd-coach.
---
```

**Obsah SKILL.md:**
- Linear MCP tools reference
- Project/Issue payload schema
- Error handling pravidla
- PRD markdown format pro description

**references/prd-template.md:**
- Kompletni PRD struktura (5 sekci)
- Priklady pro kazdy typ obsahu
- Validacni kriteria

**references/keboola-context.md:**
- Segment definitions (Enterprise, Multi-tenant, PAYG)
- Typicke metriky pro kazdy segment
- User personas (data engineers, developers, business users)
- Terminologie

---

### 3. Onboarding: INSTALL.md

**Umisteni:** `prd-workflow/INSTALL.md`

**Obsah:**

```markdown
# PRD Workflow Kit - Instalace

## Predpoklady
- Claude Code (nainstalovan a funkcni)
- Pristup do firemniho Linearu

## Krok 1: Instalace Linear MCP

```bash
claude mcp add --transport sse linear-server https://mcp.linear.app/sse
```

Pak spust Claude Code a zadej `/mcp` pro autorizaci pres Linear OAuth.

## Krok 2: Instalace PRD Workflow

```bash
# Naklonuj repo do /tmp
git clone https://github.com/keboola/prd-workflow-kit /tmp/prd-workflow-kit

# Spust instalaci
/tmp/prd-workflow-kit/scripts/install-prd-workflow.sh
```

Nebo manualne:
```bash
# Zkopiruj agenta
cp /tmp/prd-workflow-kit/prd-workflow/agents/*.md ~/.claude/agents/

# Zkopiruj skill
cp -r /tmp/prd-workflow-kit/prd-workflow/skills/prd-linear ~/.claude/skills/
```

## Krok 3: Overeni

Spust Claude Code a rekni:
> "Mam napad na novou feature - chci vytvorit PRD"

Agent keboola-prd-coach by se mel aktivovat.

## Alternativa: Keboola Claude Kit Plugin

Pokud pouzivas firemni Claude Kit, PRD workflow bude dostupny jako plugin:
```bash
/plugin marketplace add keboola/claude-kit
/plugin install product-manager
```
(Tato varianta zatim neni implementovana)
```

---

## PRD Struktura (z michal_jerabek.pdf)

### PROBLEM ALIGNMENT

**1. Problem Definition**
- Problem/opportunity description
- User Impact (data engineers, developers, business users)
- Business Impact (Enterprise retention, Multi-tenant, PAYG)
- Strategic Alignment
- What we're NOT solving

**2. High-Level Approach**
- Solution overview
- Scope indication

**3. Success Metrics**
- Enterprise: adoption, DX, retention
- Multi-Tenant/PAYG: registration, engagement, monetization
- Platform Performance
- Trade-offs

**4. Set the Baseline**
- Current metric values (with date range)
- Trend (improving/declining/stable)
- Dashboard link

### SOLUTION ALIGNMENT

**5. Key Features**
- What we're building (overview)
- What we're NOT building
- V1: Foundation / Quick Wins
- V2: Feature Completeness (optional)
- V3: Differentiation (optional)
- Decision checkpoints

---

## Validacni Kriteria

| Sekce | Povinne | Kvalitativni kontrola |
|-------|---------|----------------------|
| Problem Definition | User + Business + Strategic Impact, NOT solving | Konkretni problem, ne vague |
| High-Level Approach | Solution popis, scope | Musi navazovat na problem |
| Success Metrics | Min. 1 kategorie metrik | Konkretni cisla, ne "zlepsit" |
| Baseline | Aktualni hodnoty NEBO duvod absence | Link na dashboard pokud existuje |
| Key Features | V1 breakdown, NOT building | Jasne milestones |

---

## Linear MCP Tools

| Tool | Ucel |
|------|------|
| `mcp__linear-server__list_teams` | PM vybere cilovy team |
| `mcp__linear-server__list_projects` | Kontrola duplicit |
| `mcp__linear-server__create_project` | Vytvori projekt pro featuru |
| `mcp__linear-server__create_issue` | Vytvori issue s PRD |
| `mcp__linear-server__list_issue_labels` | Zkontroluje labels |

### Project Payload
```json
{
  "name": "[Nazev featury]",
  "description": "PRD for [nazev] - created via PRD Workflow Kit",
  "team": "[team ID]",
  "state": "planned"
}
```

### Issue Payload
```json
{
  "title": "PRD: [Nazev featury]",
  "description": "[PRD v markdown]",
  "project": "[project ID]",
  "team": "[team ID]",
  "labels": ["PRD"]
}
```

---

## Error Handling

| Situace | Reakce |
|---------|--------|
| Linear MCP neni nainstalovan | Zobraz INSTALL.md instrukce |
| Linear API timeout | Retry 1x, pak uloz PRD lokalne |
| Projekt existuje | Zeptej se: pouzit existujici nebo prejmenovat? |
| Chybejici permissions | Informuj PM, nabidni jiny team |
| PRD prilis dlouhe | Zkrat na summary + link na doc |

---

## Gap-Filling Prompty (priklady)

**User Impact chybi:**
> "Kteri uzivatele Kebooly jsou ovlivneni?
> - Data engineers (pipelines, transformace, data quality)
> - Developers (API, SDK, integrace)
> - Business users (analyzy, reporty, dashboardy)
>
> Pro kazdeho: Jaky problem resi? Jak casto?"

**Metrics jsou vague:**
> "Metrika 'zlepsit vykon' neni meritelna:
> - AKTUALNI hodnota? (napr. job duration 45s)
> - CILOVA hodnota? (napr. < 15s)
> - Jak merit? (dashboard link)"

**Baseline chybi:**
> "Nemame baseline. Moznosti:
> 1. Najit dashboard v Keboola telemetry
> 2. Pridat instrumentaci jako V0 task
> 3. Proxy metriky (support tickets, feedback)"

---

## Implementacni Kroky

### Faze 1: Priprava (tento repo)
1. Konvertovat `docs/michal_jerabek.pdf` na `docs/michal_jerabek.md`
2. Vytvorit adresarovou strukturu `prd-workflow/`
3. Napsat `INSTALL.md`

### Faze 2: Agent
1. Napsat `agents/keboola-prd-coach.md`
   - 5-fazovy workflow
   - Keboola kontext
   - Challenge mode

### Faze 3: Skill
1. Vytvorit `skills/prd-linear/SKILL.md`
2. Vytvorit `skills/prd-linear/references/prd-template.md`
3. Vytvorit `skills/prd-linear/references/keboola-context.md`

### Faze 4: Instalace
1. Napsat `scripts/install-prd-workflow.sh`
2. Otestovat na cistej instalaci

### Faze 5: Testovani
1. Test s realnym PRD napadem
2. Overit agent workflow
3. Overit Linear submission
4. Iterace podle feedbacku

---

## Budouci rozsireni

- **Claude Kit Plugin**: Zabalit jako `product-manager` plugin do keboola/claude-kit
- **PRD Templates**: Ruzne sablony pro ruzne typy features (API, UI, Platform)
- **Auto-linking**: Propojit PRD s existujicimi Linear issues
- **Metrics Dashboard**: Automaticky vyhledat relevantni dashboardy v Keboola telemetry
