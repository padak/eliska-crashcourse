# Plan: PRD Validator Skill pro Claude Code

## Cil
Vytvorit Claude Code skill, ktery:
1. Validuje PRD produktovych manazeru podle interniho standardu (docs/michal_jerabek.pdf)
2. Interaktivne vede PM pri doplneni chybejicich informaci
3. Vytvori Linear Project + Issue s PRD

---

## Struktura Skills

```
~/.claude/skills/prd-validator/
├── SKILL.md                    # Hlavni skill soubor
└── references/
    ├── prd-template.md         # Kompletni PRD sablona
    └── validation-checklist.md # Validacni pravidla
```

---

## Soubory k vytvoreni

### 1. ~/.claude/skills/prd-validator/SKILL.md

Hlavni skill obsahujici:

**Frontmatter:**
```yaml
---
name: prd-validator
description: Validace a odeslani PRD do Linearu. Pouzij pri "create PRD", "validate PRD", "submit to Linear", "PRD review".
---
```

**Obsah:**
- Prehled PRD sekci (Problem Alignment 1-4, Solution Alignment 5)
- Workflow validace (checklist pro kazdou sekci)
- Interaktivni otazky pro doplneni mezer
- Linear submission flow (team selection -> create project -> create issue)
- Formatovani PRD pro Linear description

### 2. ~/.claude/skills/prd-validator/references/prd-template.md

Kompletni PRD sablona s:
- Vsemi 5 sekcemi a jejich podsekce
- Priklady pro kazdy typ obsahu
- Otazky ktere pomahaji PM vyplnit sekci

### 3. ~/.claude/skills/prd-validator/references/validation-checklist.md

Detailni validacni pravidla:
- Co musi kazda sekce obsahovat
- Jak poznat kvalitni vs. nedostatecny obsah
- Caste chyby a jak je opravit

**Konkretni validacni kriteria (NOVE):**

| Sekce | Povinne elementy | Kvalitativni kontrola |
|-------|------------------|----------------------|
| Problem Definition | User/Business/Strategic Impact, NOT solving | Konkretni problem, ne vague |
| High-Level Approach | Popis reseni, scope | Musi navazovat na problem |
| Success Metrics | Min. 1 kategorie metrik | Konkretni cisla, ne "zlepsit" |
| Baseline | Aktualni hodnoty NEBO duvod absence | Link na dashboard pokud existuje |
| Key Features | V1 breakdown, NOT building | Jasne milestones |

---

## PRD Sekce (z docs/michal_jerabek.pdf)

### PROBLEM ALIGNMENT

**1. Problem Definition**
- Problem/opportunity description
- User Impact (data engineers, developers, business users)
- Business Impact (enterprise retention, multi-tenant, PAYG)
- Strategic Alignment
- Co NERESIME

**2. High-Level Approach**
- Strucny popis reseni
- Scope indication

**3. Success Metrics**
- Enterprise metriky (adoption, DX, retention)
- Multi-Tenant/PAYG metriky (registration, engagement, monetization)
- Platform Performance metriky
- Trade-off considerations

**4. Set the Baseline**
- Aktualni hodnoty metrik (s date range)
- Trend (improving/declining/stable)
- Link na dashboard

### SOLUTION ALIGNMENT

**5. Key Features**
- Prehled co budujeme
- Co NEbudujeme
- V1: Foundation / Quick Wins
- V2: Feature Completeness (optional)
- V3: Differentiation (optional)
- Decision checkpoints

---

## Workflow Skillu

```
1. PM spusti skill (prd-validator)
   |
2. Zjisti stav PRD:
   - Existujici dokument? -> Analyzuj
   - Od nuly? -> Provazej sekcemi
   |
3. Validace kazde sekce:
   [x] Complete  [ ] Missing  [!] Incomplete
   - Kontrola existence obsahu
   - Kontrola kvality (viz validation-checklist.md)
   |
4. Interaktivne doplneni mezer:
   - Cilene otazky pro chybejici casti
   - Navrhy a KONKRETNI PRIKLADY z Keboola kontextu
   |
5. Finalni validace:
   - Vsechny sekce OK? -> Pokracuj
   - Ne? -> Zpet k doplneni
   |
6. PREVIEW pred odeslanim (NOVE):
   - Zobrazit nazev projektu
   - Zobrazit nazev issue
   - Zobrazit strukturu PRD
   - PM potvrdi nebo upravi
   |
7. Linear submission:
   a) list_teams -> PM vybere team
   b) Kontrola duplicit (existuje projekt se stejnym nazvem?)
   c) create_project -> Novy projekt pro featuru
   d) create_issue -> Issue s PRD v description
   e) Error handling: pokud selze, zobraz chybu a nabidni retry
   |
8. Potvrzeni:
   - URL projektu
   - URL issue
   - Dalsi kroky
```

---

## Linear MCP Tools Pouzite

| Tool | Ucel |
|------|------|
| `mcp__linear-server__list_teams` | PM vybere cilovy team |
| `mcp__linear-server__list_projects` | Kontrola duplicit pred vytvorenim |
| `mcp__linear-server__create_project` | Vytvori projekt pro featuru |
| `mcp__linear-server__create_issue` | Vytvori issue s PRD |
| `mcp__linear-server__list_issue_labels` | Zkontroluje existujici labels |

---

## Linear Payload Schema (NOVE)

### Project
```
name: "[Nazev featury]"
description: "PRD for [nazev] - created via prd-validator skill"
team: [vybrane team ID]
state: "planned"
```

### Issue
```
title: "PRD: [Nazev featury]"
description: [Plny PRD v markdown - viz format nize]
project: [ID vytvoreneho projektu]
team: [vybrane team ID]
labels: ["PRD"]
```

---

## Error Handling (NOVE)

| Situace | Reakce |
|---------|--------|
| Linear API timeout | Retry 1x, pak nabidni ulozit PRD lokalne |
| Projekt se stejnym nazvem existuje | Zeptat se: pouzit existujici nebo prejmenovat? |
| Chybejici team permissions | Informovat PM, nabidnout jiny team |
| PRD prilis dlouhe pro description | Zkratit na summary + odkaz na externi doc |

---

## Linear Issue Format

**Title:** `PRD: [Nazev Projektu]`

**Description (Markdown):**
```markdown
# PRD: [Nazev Projektu]

## PROBLEM ALIGNMENT

### 1. Problem Definition
[obsah]

#### User Impact
[obsah]

#### Business Impact
[obsah]

#### Strategic Alignment
[obsah]

#### What We're NOT Solving
[obsah]

### 2. High-Level Approach
[obsah]

### 3. Success Metrics
[obsah]

### 4. Baseline
[obsah]

---

## SOLUTION ALIGNMENT

### 5. Key Features

#### Overview
[obsah]

#### What We're NOT Building
[obsah]

#### V1: Foundation / Quick Wins
[obsah]

#### V2: Feature Completeness
[obsah pokud relevant]

#### V3: Differentiation
[obsah pokud relevant]

#### Decision Checkpoints
[obsah]
```

---

## Implementacni Kroky

1. **Vytvorit adresarovou strukturu**
   ```
   mkdir -p ~/.claude/skills/prd-validator/references
   ```

2. **Vytvorit SKILL.md** (~150 radku)
   - Frontmatter s description a triggers
   - Workflow instrukce
   - Validacni logika
   - Linear submission instrukce

3. **Vytvorit prd-template.md** (~200 radku)
   - Kompletni sablona ze zdroje
   - Priklady pro kazdy typ obsahu
   - Pomocne otazky

4. **Vytvorit validation-checklist.md** (~100 radku)
   - Detailni pravidla pro kazdou sekci
   - Required vs optional elementy
   - Kvalitativni kriteria

5. **Otestovat skill**
   - Spustit s testovaci PRD ideou
   - Overit validaci
   - Overit Linear vytvoreni

---

## Gap-Filling Prompty (NOVE)

Priklady cilovych otazek pro doplneni chybejicich sekci:

**Problem Definition chybi User Impact:**
> "Kteri uzivatele Kebooly jsou timto problemem ovlivneni?
> - Data engineers (pipelines, data quality)
> - Developers (API, integrace)
> - Business users (analyzy, reporty)
>
> Pro kazdeho zmineneho popiste: Jaky problem resi? Jak casto ho maji?"

**Success Metrics jsou vague:**
> "Metrika 'zlepsit vykon' neni meritelna. Konkretizujte:
> - Jaka je AKTUALNI hodnota? (napr. job duration 45s)
> - Jaka je CILOVA hodnota? (napr. job duration < 15s)
> - Jak budete merit? (link na dashboard)"

**Baseline chybi:**
> "Nemame baseline data. Moznosti:
> 1. Najit existujici dashboard v Keboola telemetry
> 2. Pridat instrumentaci jako V0 task
> 3. Pouzit proxy metriky (support tickets, user feedback)"

---

## Poznamky

- Linear MCP nepodporuje `create_document`, proto pouzivame Issue description
- PM vybira team manualne (list_teams -> vyber)
- Skill je user-level (~/.claude/skills/), tedy globalne dostupny
- **Preview krok** pred Linear submission - PM vidi co se vytvori
- **Duplicity** se kontroluji pres list_projects pred vytvorenim
- **Error handling** zajistuje graceful degradation pri API chybach
