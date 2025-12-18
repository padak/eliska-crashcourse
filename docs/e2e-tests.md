# E2E Testy pro Elis (PRD Coach Agent)

Tento dokument obsahuje testovací scénáře pro ověření funkčnosti agenta Elis.

**Jak testovat:**
1. Restartuj Claude Code v tomto projektu
2. Spusť test řečením příslušného vstupu
3. Zaznamenej výsledek (PASS/FAIL + poznámky)

---

## 1. Entry Point Detection (Fáze 0)

### Test 1.1: Aktivace agenta jménem
**Vstup:** "Elis"
**Očekávání:** Agent se aktivuje a zeptá se "S čím přicházíš?" s nabídkou A/B/C/D
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 1.2: Nový nápad od začátku (varianta A)
**Vstup:** "Elis, mám nápad na novou feature - chci rychlejší načítání dashboardů"
**Očekávání:**
- Agent rozpozná variantu A (od začátku)
- Přejde do Discovery fáze
- Navrhne hypotézy (segment, persona)
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 1.3: Mám research, chci draft (varianta B)
**Vstup:** "Elis, mám hotový research k feature X, potřebuju pomoct napsat PRD"
**Očekávání:**
- Agent rozpozná variantu B
- Požádá o vložení obsahu researche
- Přeskočí na Drafting fázi
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 1.4: Mám draft PRD, chci review (varianta C)
**Vstup:** "Elis, mám napsaný draft PRD, potřebuju review a submit do Linearu"
**Očekávání:**
- Agent rozpozná variantu C
- Požádá o vložení draftu
- Přeskočí na Review fázi
- Zvaliduje proti šabloně (co chybí?)
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 1.5: Update existujícího PRD (varianta D)
**Vstup:** "Elis, potřebuju updatovat PRD pro projekt X v Linearu"
**Očekávání:**
- Agent rozpozná variantu D (Update Flow)
- Zeptá se na Linear URL nebo název projektu
- Spustí vyhledávání v Linearu
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 2. Hypothesis-Driven Approach (Fáze 1)

### Test 2.1: Agent navrhuje segment hypotézu
**Vstup:** "Chci přidat SLA monitoring a audit logy pro velké zákazníky"
**Očekávání:** Agent NAVRHNE: "Tohle zní jako Enterprise feature - SLA a audit log jsou typické Enterprise požadavky. Souhlasíš?"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 2.2: Agent navrhuje persona hypotézu
**Vstup:** "Potřebujeme rychlejší pipeline execution a lepší scheduling"
**Očekávání:** Agent NAVRHNE: "Předpokládám, že cílová persona jsou data engineers - pipelines a scheduling jsou jejich doména. Sedí?"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 2.3: Agent se NEPTÁ bez kontextu
**Vstup:** "Mám nápad na novou feature"
**Očekávání:** Agent se zeptá NA CO (ne formulářově "Jaký je problém?"), ale konverzačně
**Nežádoucí:** "Jaký je problem statement?" nebo jiné formulářové otázky
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 3. Challenge Patterns (Fáze 2)

### Test 3.1: Persona Mismatch
**Vstup:** "Feature pro data engineers - lepší vizualizace a reporting dashboardů"
**Očekávání:** Agent CHALLENGE: "Moment - vizualizace a dashboardy zní jako problém business users, ne data engineers. Kdo doopravdy čeká na tento výstup?"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 3.2: Segment Mismatch
**Vstup:** "Pro PAYG zákazníky chceme přidat custom deployment a SSO"
**Očekávání:** Agent CHALLENGE: "Custom deployment a SSO jsou typické Enterprise požadavky. Je PAYG správný segment? PAYG zákazníci typicky chtějí rychlý start a self-service."
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 3.3: Vague Metriky
**Vstup:** "Success metrika bude 'zlepšit developer experience'"
**Očekávání:** Agent CHALLENGE: "Jak to ZMĚŘÍŠ? Co je baseline? Jaký dashboard to ukáže? 'Zlepšit DX' není měřitelná metrika."
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 3.4: Chybějící Baseline
**Vstup:** "Chceme snížit error rate"
**Očekávání:** Agent CHALLENGE: "Jaká je AKTUÁLNÍ hodnota error rate? Bez baseline nepoznáme úspěch. Máme dashboard v Keboola telemetry?"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 3.5: Skryté Závislosti
**Vstup:** "Přidáme real-time sync pro Snowflake"
**Očekávání:** Agent CHALLENGE:
- "Podporuje Snowflake API streaming, nebo budeme muset pollovat?"
- "Real-time znamená víc API calls = vyšší náklady. Kdo platí?"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 3.6: V1 Scope Creep
**Vstup:** "V1 bude mít: import CSV, Excel, Google Sheets, Salesforce connector, a taky by bylo fajn přidat Hubspot"
**Očekávání:** Agent CHALLENGE: "Počítám 5 integrací v MVP. Co je ta JEDNA věc, bez které to nemá smysl? Zbytek můžeme přidat v V2."
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 3.7: "Quick Win" který není quick
**Vstup:** "Easy quick win - upravíme core pipeline engine"
**Očekávání:** Agent CHALLENGE: "Změny v core pipeline engine ovlivní všechny uživatele a vyžadují rozsáhlé testování. Je to opravdu quick win?"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 4. PRD Drafting (Fáze 3)

### Test 4.1: Použití PRD šablony
**Setup:** Projdi discovery a challenge fáze
**Vstup:** "Ano, pojďme draftovat PRD"
**Očekávání:**
- Agent použije strukturu z prd-template.md
- Obsahuje všech 5 sekcí (Problem Definition, Approach, Metrics, Baseline, Key Features)
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 4.2: Gap-filling s hypotézami
**Setup:** V draftu chybí User Impact
**Očekávání:** Agent NAVRHNE: "Předpokládám, že hlavní dopad je na data engineers. Souhlasíš, nebo ovlivňuje i jiné persony?"
**Nežádoucí:** Prázdná sekce nebo formulářová otázka "Vyplň User Impact"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 4.3: Validace proti Keboola kontextu
**Setup:** PRD draft hotový
**Očekávání:** Agent zvaliduje metriky, segmenty a persony proti keboola-context.md
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 5. Review & Export (Fáze 4)

### Test 5.1: Kompletní PRD preview
**Setup:** PRD draft hotový
**Vstup:** "PRD je ready"
**Očekávání:** Agent ukáže kompletní PRD ve formátovaném markdownu
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 5.2: Quality checklist
**Setup:** PRD má vague metriky
**Očekávání:** Agent upozorní: "Metriky nejsou konkrétní - chybí baseline a target hodnoty"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 5.3: Nabídka dalších kroků
**Setup:** PRD schváleno
**Očekávání:** Agent nabídne:
- Uložit jako .md soubor
- Pokračovat na Linear submit
- Pausnout
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 5.4: Export do souboru
**Vstup:** "Ulož PRD jako soubor"
**Očekávání:** Agent vytvoří .md soubor s PRD obsahem
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 6. Linear Submission (Fáze 5)

### Test 6.1: Automatické použití prd-linear skill
**Vstup:** "Submitni do Linearu"
**Očekávání:** Agent automaticky použije prd-linear skill (PM nemusí vědět že existuje)
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 6.2: Výběr týmu
**Setup:** Linear submission flow
**Očekávání:** Agent zobrazí dostupné týmy a nechá PM vybrat
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 6.3: Kontrola duplicit
**Setup:** Projekt s podobným názvem už existuje
**Očekávání:** Agent upozorní a zeptá se: "Použít existující projekt, nebo vytvořit nový?"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 6.4: Vytvoření Project + Issue
**Setup:** Linear submission potvrzeno
**Očekávání:**
- Vytvoří se Linear Project
- Vytvoří se Issue s PRD v description
- Agent vrátí URLs obou
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 6.5: Linear MCP není nainstalováno
**Setup:** Linear MCP chybí
**Očekávání:** Agent zobrazí instalační instrukce a nabídne uložit PRD lokálně
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 7. Update Flow

### Test 7.1: Najít existující PRD v Linearu
**Vstup:** "Elis, updatuj PRD pro projekt Authentication Redesign"
**Očekávání:** Agent vyhledá projekt v Linearu a zobrazí současný stav PRD
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 7.2: Update PRD obsahu
**Setup:** Existující PRD nalezeno
**Vstup:** "Změnila se scope - přidáváme OAuth support"
**Očekávání:**
- Agent aktualizuje relevantní sekce
- Přidá "Updated: [date] - [reason]" poznámku
- Ukáže diff (před → po)
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 7.3: Update Issue v Linearu
**Setup:** PRD změny schváleny
**Vstup:** "Ano, updatuj v Linearu"
**Očekávání:** Agent updatuje existující issue a přidá komentář se shrnutím změn
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 8. Pause & Resume Flow

### Test 8.1: Pausnutí uprostřed workflow
**Setup:** Uprostřed drafting fáze
**Vstup:** "Potřebuju to pausnout, vrátím se později"
**Očekávání:**
- Agent uloží draft jako .md soubor
- Řekne jak se vrátit ("řekni 'mám feedback k PRD'")
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 8.2: Návrat s feedbackem
**Setup:** Nová session, existuje uložený draft
**Vstup:** "Elis, mám feedback k PRD"
**Očekávání:**
- Agent se zeptá o který PRD jde
- Načte poslední draft
- Pokračuje v review fázi
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 8.3: Aplikace feedbacku od kolegů
**Setup:** Draft načtený
**Vstup:** "Kolega říkal, že V1 scope je moc velký"
**Očekávání:** Agent aplikuje feedback a navrhne zúžení scope
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 9. Komunikační Styl

### Test 9.1: Konverzační, ne formulářový
**Vstup:** Jakýkoli PRD nápad
**Očekávání:** Agent mluví konverzačně ("Tohle mi zní jako...", "Zajímavé, takže...")
**Nežádoucí:** Formulářové otázky ("Vyplňte problem statement", "Specifikujte segment")
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 9.2: Mix češtiny a angličtiny
**Vstup:** Komunikace v češtině
**Očekávání:** Agent mixuje přirozeně (CZ pro komunikaci, EN pro tech termíny)
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 9.3: Challenge je respektující
**Setup:** Agent challenguje nápad
**Očekávání:** Respektující tón ("Mám pochybnost...", "Uvažoval jsi o...?")
**Nežádoucí:** Agresivní nebo dismissive tón
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 9.4: Agent slaví pokrok
**Vstup:** PM poskytne konkrétní metriku
**Očekávání:** Agent ocení: "Super - tohle je konkrétní a měřitelné!"
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## 10. Edge Cases & Error Handling

### Test 10.1: PM poskytne neúplné info
**Vstup:** Velmi vágní popis bez detailů
**Očekávání:** Agent navrhne hypotézy místo série otázek
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 10.2: PM nesouhlasí s hypotézou
**Setup:** Agent navrhl segment hypotézu
**Vstup:** "Ne, to není Enterprise, je to pro PAYG"
**Očekávání:** Agent přijme korekci a upraví přístup
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 10.3: PM chce přeskočit challenge fázi
**Vstup:** "Přeskoč otázky, prostě napiš PRD"
**Očekávání:** Agent vysvětlí proč je challenge důležitá, ale respektuje rozhodnutí
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 10.4: Linear timeout
**Setup:** Linear API timeout
**Očekávání:** Agent retry 1x, pak nabídne uložit lokálně
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

### Test 10.5: PRD příliš dlouhé pro Linear
**Setup:** Velmi dlouhé PRD
**Očekávání:** Agent zkrátí na summary + link na plný dokument
**Výsledek:** ⬜ PASS / ⬜ FAIL
**Poznámky:**

---

## Shrnutí Výsledků

| Kategorie | Passed | Failed | Notes |
|-----------|--------|--------|-------|
| 1. Entry Point Detection | /5 | | |
| 2. Hypothesis-Driven | /3 | | |
| 3. Challenge Patterns | /7 | | |
| 4. PRD Drafting | /3 | | |
| 5. Review & Export | /4 | | |
| 6. Linear Submission | /5 | | |
| 7. Update Flow | /3 | | |
| 8. Pause & Resume | /3 | | |
| 9. Komunikační Styl | /4 | | |
| 10. Edge Cases | /5 | | |
| **TOTAL** | **/42** | | |

---

## Feedback & Poznámky

### Co funguje dobře:


### Co potřebuje zlepšit:


### Návrhy na úpravy agenta:


### Chybějící funkcionalita:

