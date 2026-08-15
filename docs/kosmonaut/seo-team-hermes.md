# Kosmonaut SEO-Team in Hermes

Architektur und Umsetzungsanleitung. Intern.

Wie `claude-seo`, `open-seo` und `ECC` zusammen ein SEO-Team ergeben, das in Hermes läuft
und über OpenClaw meldet.

---

## 1. Warum diese Architektur

Der naheliegende Fehler wäre, `claude-seo` und `open-seo` als Alternativen zu behandeln und
sich für eine zu entscheiden. Sie lösen verschiedene Probleme.

|                       | claude-seo v2.2.4                                                                                                       | open-seo                                                      |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Was es ist            | Methodik- und Analyse-Engine                                                                                            | Datenplattform mit Gedächtnis                                 |
| Umfang                | 25 Skills, 18 Subagents, 53 Python-Scripts                                                                              | Web-App + MCP-Server mit ~21 Tools                            |
| Datenquellen          | Live-Site (Fetch, Headless-Render, Screenshots), Google APIs (GSC, PSI, CrUX, GA4, Indexing), Moz / Bing / Common Crawl | DataForSEO, GSC, GA4 — persistiert in Projekten               |
| State                 | keiner, jeder Lauf flüchtig                                                                                             | Projekte, gespeicherte Keywords, Rank-Tracker, Audit-Historie |
| Kosten pro Lauf       | nahe null                                                                                                               | DataForSEO-Credits                                            |
| Mensch kann nachsehen | nur im Agent-Output                                                                                                     | ja, eigene UI                                                 |

**claude-seo ist das Handwerk, open-seo das Gedächtnis und die Marktdaten.**

claude-seo weiß, wie man eine Seite beurteilt, und kommt ohne Account und ohne Datenkauf an
die Wahrheit über ein Objekt, auf das ihr Zugriff habt. Was ihm strukturell fehlt: Verlauf
über Zeit, Daten über fremde Domains, und eine Oberfläche, in der ein Mensch nachprüft, was
der Agent behauptet hat. Genau das liefert open-seo.

Die dritte Komponente ist **ECC** (`affaan-m/ECC`), das Betriebssystem für Agent-Harnesses.
Es bringt den Arbeitszyklus mit — plan → test → implement → review → verify → remember →
improve — plus Memory Vault, Hooks und Cron. Entscheidend für uns: **ECC hat mit
`docs/HERMES-SETUP.md` bereits einen fertigen Hermes-Pfad.** Verzeichnislayout, Cron-Format
und Migrationstooling sind dokumentiert. Wir müssen den Adapter nicht bauen.

---

## 2. Die vier Schichten

```
OpenClaw                  Zustellung: Alerts, Reports, Rückfragen vom Handy
    ▲
    ├── Hermes            Analyse-Runtime: Orchestrator + parallele Subagents
    │      ├─ ECC         Betriebssystem: Arbeitszyklus, Memory Vault,
    │      │              Hooks, Cron, Marketing- und Content-Skills
    │      ├─ claude-seo   Fachabteilung: 25 SEO-Skills + 18 Subagents
    │      └─ open-seo     Marktdaten und Persistenz: MCP, self-hosted
    │
    └── Claude Code       Reporting-Runtime: Bestandsskills, unverändert
           └─ weekly-seo-report, searchfit-seo, brightdata-plugin
```

**Zwei Runtimes, bewusst.** Hermes ist die Analyse-Ebene, Claude Code bleibt die
Reporting-Ebene. Die Bestandsskills wandern nicht mit — `weekly-seo-report` mit seiner
ahrefs-, Sistrix-, ClickUp- und Notion-Anbindung läuft, und ein Umzug würde funktionierende
Integrationen gegen einen experimentellen Adapter tauschen.

**Hermes** trägt die neue Fachlichkeit. Sein Orchestrator-Worker-Modell mit isolierten,
parallel laufenden Subagents passt genau auf `claude-seo`s `/seo audit`, das bis zu 15
Subagents gleichzeitig startet. Das ist kein Zufallstreffer, sondern der Grund, warum die
Kombination trägt.

**OpenClaw** bekommt kein Skill-Set. Es stellt zu, mehr nicht — und ist damit zugleich die
einzige Stelle, an der beide Runtimes zusammenlaufen. Wer OpenClaw mit Skills bestückt, hat
eine dritte Wahrheit darüber, wie ein Audit läuft.

### Rollen im Team

| Rolle                    | Wer                   | Aufgabe                                                   |
| ------------------------ | --------------------- | --------------------------------------------------------- |
| Teamleitung              | Hermes-Orchestrator   | zerlegt Aufträge, verteilt, führt zusammen                |
| Arbeitsweise             | ECC                   | erzwingt Plan vor Bau und Review aus frischem Kontext     |
| Gedächtnis (qualitativ)  | ECC Memory Vault      | Kundenkontext, Präferenzen, Historie                      |
| Gedächtnis (quantitativ) | open-seo              | Keywords, Rankings, Audit-Läufe                           |
| SEO-Fachlichkeit         | claude-seo            | 18 Spezialisten von technisch bis GEO                     |
| Text und Marke           | ECC-Marketing-Skills  | Artikel, Brand Voice, Repurposing                         |
| Reporting                | Claude Code (Bestand) | Wochenbericht, ClickUp, Notion — bleibt wo es ist         |
| Meldewesen               | OpenClaw              | Alerts und Reports zustellen, Klammer über beide Runtimes |

---

## 3. Routing

Der wichtigste Teil dieses Dokuments. Ohne diese Regeln zahlt ihr DataForSEO-Credits für
Dinge, die claude-seo kostenlos kann.

**Faustregel: kostenlos vor kostenpflichtig, eigenes Objekt vor Fremddaten, flüchtig vor
persistent.** open-seo wird gerufen, wenn eine Entscheidung Historie oder Daten über eine
fremde Domain braucht — nicht als Standardeinstieg.

| Aufgabe                                                       | Zuständig                                                | Warum                                           |
| ------------------------------------------------------------- | -------------------------------------------------------- | ----------------------------------------------- |
| On-Page, technisch, Schema, Core Web Vitals, Bilder, hreflang | claude-seo                                               | arbeitet am Live-Objekt, kostenlos              |
| GSC / GA4 / CrUX für eigene Properties                        | claude-seo (`/seo google`)                               | direkter API-Zugriff, keine Credits             |
| Keyword-Universum, Volumen, KD, SERP-Rows                     | open-seo                                                 | DataForSEO, wird persistiert                    |
| Rankings über Zeit                                            | open-seo                                                 | einzige Schicht mit Historie                    |
| Competitor-Footprint (fremde Domain)                          | open-seo                                                 | fremde GSC ist für uns unsichtbar               |
| Backlinks, erste Näherung                                     | claude-seo (Moz / Bing / Common Crawl)                   | kostenlos, reicht zur Triage                    |
| Backlinks, belastbar                                          | open-seo                                                 | wenn die Näherung eine Entscheidung tragen soll |
| Content-Brief, Cluster, GEO, SXO                              | claude-seo                                               | Methodik, kein Datenkauf                        |
| Texte, Brand Voice, Repurposing                               | ECC (`article-writing`, `brand-voice`, `content-engine`) | dafür gebaut                                    |
| Kundenkontext und Präferenzen                                 | ECC Memory Vault                                         | `.ecc/memory/`                                  |
| Wochenreport, ClickUp, Notion                                 | Bestand (`weekly-seo-report`)                            | läuft, nicht anfassen                           |

### Namenskollisionen

Fünf Skills triggern auf „SEO":

| Skill                                  | Runtime     | Umgang                                     |
| -------------------------------------- | ----------- | ------------------------------------------ |
| `searchfit-seo:seo-audit`              | Claude Code | Bestand, bleibt                            |
| `brightdata-plugin:seo-audit`          | Claude Code | Bestand, bleibt                            |
| `claude-seo` → `/seo audit`            | Hermes      | Standard für On-Page und Technik           |
| `open-seo` → `seo-audit`               | Hermes      | nur bei Bedarf an Historie oder Fremddaten |
| `ECC` → `seo` (aus `business-content`) | Hermes      | **abschalten oder umbenennen**             |

Ein Agent, der frei wählen darf, wählt hier zufällig. ECCs `seo` ist der unangenehmste Fall:
er kommt als Beifang mit dem Marketing-Modul und deckt fachlich nichts ab, was claude-seo
nicht besser kann. Entweder nach der Installation aus `~/.hermes/skills/` entfernen, oder
seine `description` so entschärfen, dass sie nicht mehr auf SEO-Anfragen triggert.

Die Runtime-Trennung entschärft den Rest zur Hälfte: die beiden Bestandsskills leben in
Claude Code, die neuen in Hermes. Innerhalb von Hermes bleibt die Wahl zwischen claude-seo
und open-seo — dafür der Router-Skill `kosmonaut-seo` als einziger Einstiegspunkt, der die
Tabelle in Abschnitt 3 als Entscheidungslogik trägt.

Was der Router **nicht** kann: über die Runtime-Grenze rufen. Ein Auftrag, der Analyse und
Wochenbericht verbindet, braucht zwei Aufrufe in zwei Runtimes. Die Klammer ist OpenClaw
oder ihr selbst — nicht der Router.

---

## 4. Installation

### Schritt 1 — ECC in Hermes

```bash
npm install -g ecc-universal
ecc memory init --scope project --scope team
command -v ecc-memory-mcp     # muss einen Pfad liefern
```

Verzeichnislayout danach:

| Pfad                            | Inhalt                                    |
| ------------------------------- | ----------------------------------------- |
| `~/.hermes/config.yaml`         | Model-Routing, MCP-Registrierung, Plugins |
| `~/.hermes/skills/ecc-imports/` | ECC-Skills für Hermes                     |
| `~/.hermes/plugins/`            | Hook-Bridges                              |
| `~/.hermes/cron/jobs.json`      | geplante Läufe mit explizitem Prompt      |
| `<repo>/.ecc/memory/`           | Projekt- und Team-Kontext                 |
| `~/.ecc/memory/`                | nutzerweiter Kontext über Repos hinweg    |

Existiert bereits ein `~/.hermes`, vorher den Migrationspfad fahren:

```bash
ecc migrate audit --source ~/.hermes
ecc migrate plan
ecc migrate import-skills --output-dir migration-artifacts/skills
ecc migrate import-memory
```

### Schritt 2 — Nur die gewünschten Module installieren

```bash
./install.sh --target hermes --profile minimal \
    --with business-content --with skill-unified-memory
```

Das ist genau „ECC-Kern plus Marketing":

| Teil                   | Was drinsteckt                                                                                                                                                                                                                                                                                      |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `minimal`              | `rules-core`, `agents-core`, `commands-core`, `platform-configs`, `workflow-quality`                                                                                                                                                                                                                |
| `business-content`     | `article-writing`, `brand-voice`, `brand-discovery`, `content-engine`, `market-research`, `marketing-campaign`, `competitive-platform-analysis`, `competitive-report-structure`, `lead-intelligence`, `social-graph-ranker`, `investor-materials`, `investor-outreach`, `product-capability`, `seo` |
| `skill-unified-memory` | Memory Vault — trägt die qualitative Gedächtnisschicht                                                                                                                                                                                                                                              |

`skill-unified-memory` ist bewusst dazugenommen: der Memory Vault ist in dieser Architektur
die Ablage für Kundenkontext, steckt aber **nicht** in `minimal`.

> **Zur Quellenlage.** `docs/SELECTIVE-INSTALL-ARCHITECTURE.md` liest sich, als gäbe es
> selective install noch nicht — das Dokument beschreibt einen späteren Ausbau, nicht den
> Ist-Stand. Maßgeblich ist `scripts/install-apply.js`: dort werden `--profile`, `--target`,
> `--modules`, `--skills`, `--with` und `--without` geparst, und `hermes` wie `openclaw` sind
> unterstützte Targets. Die Design-Doku taugt nicht als Statusquelle.

`business-content` bringt einen eigenen Skill namens `seo` mit — siehe Namenskollisionen in
Abschnitt 3.

### Schritt 3 — claude-seo als Fachabteilung

claude-seo ist auf genau diesen Fall ausgelegt. `AGENTS.md` dokumentiert die
Cross-Harness-Portabilität, `portability_check.py` verifiziert sie.

```bash
cd claude-seo
./bin/claude-seo run portability_check.py    # Frontmatter-Kompatibilität prüfen
cp -r skills/* ~/.hermes/skills/             # 25 Skills
cp -r agents/* ~/.hermes/agents/             # 18 Subagents, Pfad an Hermes anpassen
./bin/claude-seo setup                       # isoliertes Python-Venv + Chromium
```

Die Python-Scripts laufen **immer** über `claude-seo run <script>`, nie über einen nackten
Interpreter. Der Launcher hält das Venv isoliert; ein `pip install` daneben bricht das.

### Schritt 4 — open-seo self-hosted und als MCP eintragen

**Entschieden: Cloudflare.** Deployment nach `docs/SELF_HOSTING_CLOUDFLARE.md`,
DataForSEO-Key nach `docs/DATAFORSEO_API_KEY.md`. Damit läuft die Instanz mit
`AUTH_MODE=cloudflare_access` — echte Zugriffskontrolle über Cloudflare Access, dazu
API-Keys für alles Headless.

> **Warum nicht Docker.** Der Docker-Pfad setzt `AUTH_MODE=local_noauth`: keine
> Auth-Prüfung am MCP-Endpoint, Nutzer ist immer `admin@localhost`. Wer den Port erreicht,
> hat vollen Zugriff auf alle Kundenprojekte und verbrennt das DataForSEO-Guthaben. Für
> lokale Tests ohne Kundendaten in Ordnung, für Mandate nicht.

API-Keys tragen das Präfix `oseo_` und gehen als `x-api-key`-Header oder als
`Authorization: Bearer oseo_…`. Für Cron und headless **immer API-Key statt OAuth** — der
OAuth-Flow braucht einen Browser, den ein Cronjob nicht hat.

Danach den MCP-Endpoint der eigenen Instanz (Pfad `/mcp`) in `~/.hermes/config.yaml` bzw.
`mcp-configs/mcp-servers.json` eintragen.

### Schritt 5 — Router-Skill

`kosmonaut-seo` als SKILL.md in `~/.hermes/skills/`. Inhalt: die Routing-Tabelle aus
Abschnitt 3 als Entscheidungslogik, plus explizite Abgrenzung gegen die Bestandsskills.

### Schritt 6 — OpenClaw als Meldekanal

Hermes-Cron (`~/.hermes/cron/jobs.json`) führt aus, OpenClaw stellt zu. Zwei Kandidaten für
den Start: `/seo drift compare` täglich je Kundendomain, und die Zustellung des bestehenden
Wochenreports.

> **Zwingend:** Jede Harness braucht ihren eigenen `ecc-memory-mcp`-Prozess mit eigener
> `ECC_MEMORY_HARNESS`-Identität. Hermes und OpenClaw dürfen sich **nicht** denselben
> Serverprozess teilen.

Nutzerweiter Memory-Zugriff erfordert zusätzlich `ECC_MEMORY_ALLOW_USER_SCOPE=1`.

---

## 5. Betrieb

| Wann          | Was                                  | Runtime         | Wer führt aus          | Wer bekommt es               |
| ------------- | ------------------------------------ | --------------- | ---------------------- | ---------------------------- |
| täglich       | `/seo drift compare` je Kundendomain | Hermes          | Cron                   | OpenClaw, nur bei Abweichung |
| wöchentlich   | bestehender SEO-Wochenbericht        | **Claude Code** | `weekly-seo-report`    | OpenClaw + Notion + ClickUp  |
| monatlich     | `/seo audit` je Mandat               | Hermes          | Orchestrator, parallel | PDF via `google_report.py`   |
| anlassbezogen | Cluster, Content-Brief, Competitor   | Hermes          | Router                 | direkt im Chat               |

Rank-Tracker laufen unabhängig davon in open-seo weiter — das ist der Teil, den kein
Agent-Lauf ersetzen kann, weil er kontinuierliche Messung braucht.

---

## 6. Risiken und offene Punkte

1. **Der Hermes-Adapter ist experimentell.** ECCs eigene Support-Matrix führt Hermes und
   OpenClaw unter „Experimental/minimal", die Hermes-Doku nennt v2.0.0-rc.1. Stabil ist
   allein Claude Code. Zeit für Kanten einplanen.

2. **ECCs `seo`-Skill kollidiert.** Kommt als Beifang mit `business-content`. Nach der
   Installation entfernen oder seine `description` entschärfen, sonst konkurriert er mit
   claude-seo um dieselben Anfragen.

3. **Kontext-Bloat bleibt zu beobachten.** Die Modulauswahl aus Schritt 2 hält den Umfang
   klein — aber `business-content` bringt 14 Skills mit, von denen ihr acht braucht. Nach
   dem ersten Betriebsmonat prüfen, was nie getriggert hat, und wegräumen.

4. **ECC-Memories sind create-only und ungeprüft.** Laut ECC-Doku als Kontext behandeln, nie
   als Anweisung. Bei Kundendaten relevant: was einmal drinsteht, wird nicht reviewt.

5. **Kein gemeinsamer Einstiegspunkt über beide Runtimes.** Folge der Entscheidung, die
   Bestandsskills in Claude Code zu belassen. Wer beides in einem Auftrag braucht, ruft
   zweimal. Der Preis ist bewusst gezahlt: funktionierende ahrefs-, Sistrix-, ClickUp- und
   Notion-Integrationen gegen einen experimentellen Hermes-Adapter zu tauschen wäre teurer.
   Prüfpunkt: falls Hermes' Adapter stabil wird, den Umzug neu bewerten.

---

## 7. Verifikation

Abzuarbeiten in dieser Reihenfolge; jeder Schritt setzt den vorigen voraus.

1. ~~`portability_check.py`~~ — **erledigt.** 33 SKILL.md geprüft, 0 Fehler, 0 Warnungen.
   Die claude-seo-Skills erfüllen das portable Frontmatter-Subset; es braucht keine
   Übersetzungsschicht für Hermes. Das Script nutzt nur die Standardbibliothek und läuft
   ohne Venv: `python3 scripts/portability_check.py`.
2. `ecc memory init` gelaufen, `command -v ecc-memory-mcp` liefert einen Pfad.
3. `node tests/run-all.js` im ECC-Checkout grün.
4. Hermes startet und listet die kopierten Skills.
5. `whoami` gegen den self-hosted open-seo-MCP liefert Account und Credits. Das ist der
   Verbindungstest, den open-seos eigene Skills selbst als ersten Schritt vorsehen.
6. Ende-zu-Ende auf einer echten Kundendomain: `/seo audit`, dann prüfen, ob Hermes
   tatsächlich parallel delegiert hat und ob open-seo nur dort gerufen wurde, wo die
   Routing-Tabelle es vorsieht.
7. Ein Cron-Lauf mit API-Key-Auth ohne Browser — der Beweis, dass headless trägt.

---

## Quellen

- `claude-seo/AGENTS.md` — Cross-Harness-Portabilität, Tool-Namens-Mapping
- `claude-seo/skills/seo/SKILL.md` — Kommandotabelle, Orchestrierungslogik
- `open-seo/docs/mcp.md` — MCP-Endpoint und Client-Setup
- `open-seo/docs/SELF_HOSTING_DOCKER.md` — `AUTH_MODE=local_noauth`
- `open-seo/src/server/mcp/api-key-auth.ts` — `oseo_`-Präfix, Header-Varianten
- `ECC/docs/HERMES-SETUP.md` — Hermes-Layout, Cron, Memory-Vault-Regeln
- `ECC/docs/SELECTIVE-INSTALL-ARCHITECTURE.md` — Designstatus des Selective Install
