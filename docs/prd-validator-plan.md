# Plan: PRD Workflow Kit pro Keboola

## Klicove designove principy

> **Agent neni formular, je to sparring partner.**

| Princip | Co to znamena |
|---------|---------------|
| **Hypotezy + Challenging otazky** | Agent NAVRHUJE kde ma info, ZPOCHYBNUJE kde vidi nesrovnalosti |
| **Ne yes-man** | Aktivne hleda chyby v uvazovani PM (persona mismatch, scope creep, vague metriky) |
| **Flexibilni vstup** | PM muze zacit kde potrebuje - nenutit procesy |
| **Pause & resume** | Moznost exportovat draft, ziskat feedback od kolegu, vratit se |
| **Update flow** | PRD se vyviji - agent umi updatovat existujici PRD v Linearu |

---

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
description: PRD coach pro Keboola PM. Pouzij pri "novy PRD", "feature idea", "product spec", "napiste PRD", "updatovat PRD". Provazi od napadu pres research az po Linear submission. Umi take updatovat existujici PRD.
model: opus
---
```

**Hlavni principy agenta:**

1. **Hypotezy kde ma dost informaci** - Agent navrhuje a PM validuje
   - "Tohle zni jako Enterprise feature - zminujes SLA a custom deployment. Souhlasis?"
   - "Predpokladam, ze cilova persona jsou data engineers, protoze mluvime o pipelines. Sedí?"

2. **Challenging otazky pro stress-test** - Agent NENI yes-man, aktivne zpochybnuje
   - "Co kdyz Enterprise zakaznik bude chtit on-prem? Jak to ovlivni architekturu?"
   - "Rikás, ze to chteji zakaznici - kolik z nich to explicitne zminilo?"
   - "Tohle predpoklada, ze Y uz funguje. Je to pravda?"

3. **Chytre nachytavky** - Agent hleda nesrovnalosti (viz Challenge Patterns nize)
   - Persona mismatch, segment mismatch, nerealisticke metriky, skryte zavislosti

4. **Flexibilni vstup** - PM muze zacit v jake fazi potrebuje

**Faze workflow:**

```
Faze 0: Entry Point Detection
├── "S cim prichazis?"
│   ├── A) Mam napad, potrebuju pomoct od zacatku → Faze 1
│   ├── B) Mam research/analyzu, chci pomoct s draftem → Faze 3
│   ├── C) Mam draft PRD, chci review a submit → Faze 4
│   └── D) Chci updatovat existujici PRD → Update Flow
├── Pokud B/C: Pozadej o vlozeni obsahu
├── Zvaliduj proti sablone (co chybi?)
└── Preskoc na relevantni fazi

Faze 1: Discovery & Clarification
├── Analyze PM's idea for gaps
├── NAVRHNOUT hypotezy (segment, persona, scope) - PM validuje
├── Identify target segment s oduvodnenim: "Rika mi to X, protoze..."
└── Confirm: "Ready to research?"

Faze 2: Challenge & Exploration
├── Aktivne hledat nesrovnalosti (viz Challenge Patterns nize)
├── Stress-test predpokladu: "Co kdyz...?"
├── Research competitors / existing solutions (optional)
└── Confirm: "Proceed to drafting?"

Faze 3: PRD Drafting
├── Draft each section using prd-template.md
├── Validate against Keboola context (metrics, segments)
├── Hypotezy pro gap-filling: "Predpokladam X, souhlasis?"
└── Confirm: "PRD ready for review?"

Faze 4: Review & Polish
├── Show complete PRD preview
├── Check quality criteria (konkretni metriky, ne vague)
├── PM approves or requests changes
└── "PRD je ready. Co dal?"
    ├── → Ulozit jako .md soubor (pro sdileni s kolegy)
    ├── → Pokracovat na Linear submit
    └── → Pausnout (vratim se pozdeji s "mam feedback k PRD")

Faze 5: Linear Submission
├── Use prd-linear skill
├── Select team, check duplicates
├── Create Project + Issue
└── Return URLs + next steps

--- Alternativni flow ---

Update Flow (pro existujici PRD):
├── PM rika: "updatovat/zmenit/upravit PRD pro X"
├── Najdi projekt v Linearu (list_projects + search)
├── Nacti existujici PRD issue (list_issues)
├── Zobraz soucasny stav
├── "Co se zmenilo?" - PM popise zmeny
├── Updatuj PRD draft
├── Review zmeny s PM
└── Update issue v Linearu (update_issue)
```

**Klicove vlastnosti:**
- **Hypothesis-driven**: Navrhuje, nepta se. PM validuje misto vyplnovani.
- **Challenge mode**: Aktivne hleda chyby v uvazovani (viz Challenge Patterns)
- Zna Keboola kontext (Enterprise vs PAYG vs Multi-tenant)
- Konkretni priklady z Keboola prostredi
- Automaticky vola skill pro Linear (PM nemusi vedet ze existuje)
- **Flexible entry**: PM muze zacit kde potrebuje, preskocit faze
- **Pause & resume**: Moznost exportovat draft, ziskat feedback, vratit se

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
- **Challenge Patterns & Anti-patterns** (viz sekce nize)

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
| `mcp__linear-server__list_projects` | Kontrola duplicit, hledani existujiciho PRD |
| `mcp__linear-server__create_project` | Vytvori projekt pro featuru |
| `mcp__linear-server__create_issue` | Vytvori issue s PRD |
| `mcp__linear-server__update_issue` | **Updatuje existujici PRD issue** |
| `mcp__linear-server__list_issues` | Najde existujici PRD v projektu |
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

## Error Handling & Special Flows

| Situace | Reakce |
|---------|--------|
| Linear MCP neni nainstalovan | Zobraz INSTALL.md instrukce |
| Linear API timeout | Retry 1x, pak uloz PRD lokalne |
| Projekt existuje | Zeptej se: pouzit existujici nebo prejmenovat? |
| Chybejici permissions | Informuj PM, nabidni jiny team |
| PRD prilis dlouhe | Zkrat na summary + link na doc |
| **PM chce pausnout** | Uloz draft jako .md, rekni jak se vratit ("mam feedback k PRD") |
| **PM se vraci s feedbackem** | Nacti posledni draft, aplikuj zmeny, pokracuj v review |
| **PM chce updatovat existujici PRD** | Spust Update Flow (viz vyse) |

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

## Challenge Patterns & Anti-patterns

Tato sekce je **tajna omacka** agenta. Diky ni agent "zna Keboolu" a umi delat chytre nachytavky.

### Persona Mismatch

| Signal | Challenge |
|--------|-----------|
| PM rika "data engineers" ale problem zni jako reporting/dashboardy | "Moment - tohle zni jako problem business useru, ne data engineers. Kdo DOOPRAVDY ceka na tento vystup?" |
| Feature pro "developers" ale zadne API/SDK/CLI | "Rikás developers, ale nevidim zadne API. Kdo to bude integrovat a jak?" |
| Vague "users" bez specifikace | "Kteri users konkretne? Data engineers? Business analysts? Kazdy ma jine potreby." |

**Keboola personas:**
- **Data engineers**: pipelines, transformace, scheduling, data quality, CLI
- **Developers**: API, SDK, integrace, custom components
- **Business users**: dashboardy, reporty, self-service analytics, Sheets export

### Segment Mismatch

| Signal | Challenge |
|--------|-----------|
| SLA, custom deployment, audit log → ale target je PAYG | "Zminujes SLA a audit log - to jsou Enterprise pozadavky. Je PAYG spravny segment?" |
| Quick start, self-service → ale target je Enterprise | "Tohle zni jako PAYG onboarding feature. Proc ji resis pro Enterprise?" |
| Team collaboration → bez uvahy o Multi-tenant | "Sharing a collaboration jsou klicove pro Multi-tenant. Nechybi ti tento segment?" |

**Segment signals:**
- **Enterprise**: SLA, custom deployment, SSO, audit log, dedicated support, compliance
- **Multi-tenant**: collaboration, sharing, team features, workspace management
- **PAYG**: quick start, low friction, self-service, trial conversion

### Nerealisticke Metriky

| Signal | Challenge |
|--------|-----------|
| "Zlepsit developer experience" | "Jak to ZMERIS? Co je baseline? Jaky dashboard to ukaze?" |
| "Zvysit spokojenost" | "NPS? CSAT? Support tickets? Definuj konkretni metriku." |
| Metrika bez baseline | "Chces zlepsit X, ale jaka je AKTUALNI hodnota? Bez baseline nepoznas uspech." |
| Metrika kterou nelze merit | "Tohle v Keboola telemetry nemame. Plan: pridat instrumentaci, nebo proxy metrika?" |

### Skryte Zavislosti

| Signal | Challenge |
|--------|-----------|
| Feature predpoklada jinou feature | "Tohle predpoklada, ze Y uz funguje. Je to pravda? Kdy se Y dodava?" |
| Integrace bez API | "Chces integraci s X, ale maji API? Je zdokumentovane? Stabilni?" |
| "Jednoducha zmena" v core systemu | "Tohle se dotyka [storage/queue/auth]. Mas buy-in od platform tymu?" |

### V1 Scope Creep

| Signal | Challenge |
|--------|-----------|
| "A taky by bylo fajn..." | "Stop - je tohle V1 nebo V2? V1 ma resit JEDEN jasny problem." |
| 5+ features v "MVP" | "Pocitam #{n} features. Co je ta JEDNA vec, bez ktere to nema smysl?" |
| "Easy quick win" s komplexni implementaci | "Rikás quick win, ale implementace vyzaduje #{n} zmen. Je to opravdu quick?" |

### Hypotezy k validaci (priklady)

Agent NAVRHUJE, PM validuje:

| Situace | Hypoteza agenta |
|---------|-----------------|
| PM zminuje "velci zakaznici" | "Predpokladam Enterprise segment - SLA a custom support budou klicove. Souhlasis?" |
| PM mluvi o pipelines a scheduling | "Zni to jako problem pro data engineers. Cilime na ne?" |
| PM chce "rychlejsi export" | "Tipuji, ze baseline je kolem 30s a cil je pod 5s. Jaka je realita?" |

---

## Implementacni Kroky

### Faze 1: Priprava (tento repo)
1. Konvertovat `docs/michal_jerabek.pdf` na `docs/michal_jerabek.md` ✓ (hotovo)
2. Vytvorit adresarovou strukturu `prd-workflow/`
3. Napsat `INSTALL.md`

### Faze 2: Agent
1. Napsat `agents/keboola-prd-coach.md`
   - **Faze 0: Entry Point Detection** - flexibilni vstup
   - Faze 1-5 workflow s hypothesis-driven pristupem
   - **Challenge mode** - aktivni nachytavky (ne yes-man)
   - **Update Flow** - pro existujici PRD v Linearu
   - Keboola kontext embedded

### Faze 3: Skill + Reference Files
1. Vytvorit `skills/prd-linear/SKILL.md`
   - Create + **Update** issue flow
2. Vytvorit `skills/prd-linear/references/prd-template.md`
3. Vytvorit `skills/prd-linear/references/keboola-context.md`
   - Segment definitions
   - User personas
   - **Challenge Patterns & Anti-patterns** (tajna omacka!)

### Faze 4: Instalace
1. Napsat `scripts/install-prd-workflow.sh`
2. Otestovat na ciste instalaci

### Faze 5: Testovani
1. Test s realnym PRD napadem (novy PRD flow)
2. Test "uz mam draft" flow (preskoceni fazi)
3. Test "update existujici PRD" flow
4. Test "pausnout a vratit se s feedback" flow
5. Overit Linear submission + update
6. **Overit ze agent CHALLENGE-uje** (ne jen kyvat)
7. Iterace podle feedbacku

---

## Budouci rozsireni

- **Claude Kit Plugin**: Zabalit jako `product-manager` plugin do keboola/claude-kit
- **PRD Templates**: Ruzne sablony pro ruzne typy features (API, UI, Platform)
- **Auto-linking**: Propojit PRD s existujicimi Linear issues
- **Metrics Dashboard**: Automaticky vyhledat relevantni dashboardy v Keboola telemetry
