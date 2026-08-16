# Von Tools zu Entscheidungen: Agenten-Teams für Kosmonaut-SEO

Strategie-Ergänzung zum [Konzept](./seo-team-hermes.md) und [Masterplan](./plan/README.md).
Stand 16.08.2026, Rev. 3 nach drei Codex-Review-Zyklen.
Die Technik (M1 fertig, M2 delegiert an die Hermes-Session, M3 vorbereitet) beantwortet
„womit". Dieses Dokument beantwortet „wer entscheidet was, auf welcher Datenbasis,
mit wie viel Autonomie".

## 1. Das Ziel in einem Satz

Eigenständige Agenten-Teams in Hermes (orchestriert über Paperclip) holen Daten aus
den angebundenen Quellen, analysieren und **verifizieren** sie, und leiten daraus
Marketing- und SEO-Maßnahmen ab — der Mensch genehmigt Maßnahmen, er produziert
sie nicht.

## 2. Datenquellen-Matrix (Ist-Stand 16.08.2026)

| Quelle | Daten | Zugang | Status |
| --- | --- | --- | --- |
| open-seo (selfhost) | Keywords, SERP, Rankings über Zeit, Backlinks (belastbar), Audit-Historie, DataForSEO | MCP `https://openseo.tiaex.ai/mcp` + 7 Skills (`~/.claude/skills/`) | **live**, M3 verdrahtet es in Hermes |
| Google Search Console | Queries, Klicks, Impressionen, Positionen, Indexierung | (a) Service-Account via claude-seo (`gsc_query.py`, 4 Properties), (b) OAuth-Connect im open-seo-Projekt „KOSMONAUT Germany" | **live** |
| claude-seo | On-Page, Technik, Schema, CWV, GEO, Local, Drift — 25 Skills, 18 Agents, 53 Scripts | Skills direkt in Hermes (M2) | M2 delegiert |
| Screaming Frog v24+ | Voll-Crawls, Bulk-Exports, URL-Inspektion, Screenshots | nativer MCP-Server (~29 Tools, headless-fähig) | anzubinden (M6b, Preflight nötig: Maschine + Lizenz) |
| Ahrefs | Backlinks, organische Keywords, Content | offizieller Ahrefs-MCP (claude-seo-Extension `/seo ahrefs`) | Bestand/Ergänzung — siehe Routing-Hinweis unten |
| Sistrix | Sichtbarkeitsindex, Wettbewerb | Bestand `weekly-seo-report` (Claude Code) | **bleibt im Bestand**; Migration nur mit eigenem Business-Case |
| Google Analytics 4 | Traffic, Conversions, Verhalten | open-seo-Connect (gleicher OAuth-Client, `/api/ga4/oauth/callback`) + claude-seo `ga4_report.py` | vorbereitet, bewusst später |

**Routing-Hinweis (bindend):** Die Routing-Tabelle des Konzepts gilt unverändert —
kostenlos vor kostenpflichtig, eigenes Objekt vor Fremddaten, flüchtig vor
persistent. Belastbare Backlink-Entscheidungen laufen über **open-seo**; Ahrefs ist
Zusatz-Evidenz mit Kosten-Gate, keine Hermes-Primärquelle. Sistrix bleibt in der
Reporting-Runtime (Claude Code).

## 3. Die Entscheidungs-Pipeline (der Kern)

```
SCOPE → FRISCHE/TTL-CHECK → KOSTENSCHÄTZUNG → BUDGET-RESERVIERUNG → LOW-TRUST-GATE
  → HOLEN → ANALYSIEREN → VERIFIZIEREN → MASSNAHMEN-KARTE → FREIGEBEN
  → AUSFÜHREN (Lock + Idempotenz) → MESSEN
```

Die fünf Vorstufen sind neu gegenüber Rev. 1 und **fail-closed**:

1. **Scope:** Mandat (`company_id`), Objekt (Domain/URL-Set), Fragestellung. Ohne
   Scope keine Tool-Calls.
2. **Frische/TTL:** Liegt eine ausreichend frische Antwort im Evidence-Store
   (open-seo-Historie, ECC-Memory, letzter Crawl)? TTL je Datenart: SERP/Rankings 7
   Tage, Backlink-Indizes 30 Tage, GSC 1 Tag, Crawls 30 Tage oder bis Deploy-Event.
   Frisch genug → HOLEN entfällt.
3. **Kostenschätzung:** Jeder kostenpflichtige Call (DataForSEO via open-seo, Ahrefs,
   Sistrix) wird vorab geschätzt (open-seo bietet `Estimate rank check cost`).
4. **Budget-Reservierung:** Schätzung wird gegen das Paperclip-Budget der Company
   reserviert. Keine Reservierung → kein Call. Kostenlose Quellen (GSC, claude-seo,
   eigener Crawl) passieren frei.
5. **Low-Trust-Gate:** Alles, was Fremdinhalte einliest (SERPs, fremde Websites,
   Backlink-Ziele), läuft im Low-Trust-Preset von Paperclip (Sandbox-Containment,
   Prompt-Injection-Annahme). Fremdtext ist Daten, nie Anweisung.

### 3.1 Evidenzmatrix (versioniert, v1)

Jeder Befundtyp hat einen definierten Bestätigungsmodus. Drei Modi:
**Z** = unabhängige Zweitquelle, **R** = direkte Reproduktion am Live-Objekt,
**S** = Single-Source genügt, aber Maßnahme braucht Human-Approval.

| Befund-Typ | Primärquelle | Modus | Bestätigung |
| --- | --- | --- | --- |
| Ranking-Verlust | open-seo Rank-Tracker | Z | GSC-Position derselben Query (gleiches Land/Gerät, gleicher Zeitraum) |
| Backlink-Risiko/-Chance | open-seo | Z* | Ahrefs oder Moz/CommonCrawl; *Index-Abweichungen sind normal — bestätigt ist ein Befund, wenn die RICHTUNG übereinstimmt, nicht die absolute Zahl |
| Technik-Problem (Status, Redirects, interne Links) | Screaming-Frog-Crawl | R | claude-seo `/seo technical` reproduziert am Live-Objekt |
| Schema-Fehler | claude-seo `/seo schema` | R | Zweitvalidierung (Rich-Results-Test via Script) |
| CWV-Verschlechterung | PSI/CrUX (claude-seo) | Z | CrUX-Felddaten vs. Lab-Messung; Feld schlägt Lab |
| Indexierungs-Problem | GSC URL-Inspection | S | Single-Source ok (Google ist hier die Wahrheit), Maßnahme mit Approval |
| Content-Lücke | open-seo Keyword-/SERP-Daten | Z | GSC: Query hat Impressionen ohne adäquate Seite ODER Wettbewerber-SERP-Beleg; reine Volumen-Behauptung reicht nicht |
| Traffic-Anomalie | GA4 | Z | GSC-Klicks im selben Segment + open-seo-Drift-Baseline; nur gleichgerichtete Signale gelten |
| GEO/Local-Befund | claude-seo `/seo geo`, `/seo local` | S | Single-Source + Approval (keine belastbare Zweitquelle vorhanden) |

Widersprechen sich Quellen über die Richtung, eskaliert der Verifizierer an den
Menschen — mit beiden Datenständen, nicht mit einer Meinung.

### 3.2 Evidence-Bundle und Maßnahmen-Karte

**Evidence-Bundle** (Übergabeformat zwischen Rollen, versioniert, im open-seo-Projekt
bzw. R2 abgelegt): Bundle-ID, `company_id`, Scope, Quelle+Abfragezeit+Parameter
(Land, Gerät, Zeitraum), Rohdaten-Verweis, TTL. Der Verifizierer arbeitet NUR auf
Bundles — er holt nichts erneut.

**Evidence-Store-Regeln (fail-closed):** Die Trennung je `company_id` wird
**speicherseitig erzwungen**, nicht nur vereinbart: je Company ein eigener
R2-Bucket oder ein prefix-gebundenes API-Token (R2 Scoped Credentials) bzw. in
open-seo die Projekt-Bindung des MCP-Zugangs — ein Worker BESITZT technisch
keinen Zugriff auf fremde Namespaces. Abnahmekriterium in M6a: ein
Cross-Company-Leseversuch schlägt im Test technisch fehl. Der Kill-Switch
(Abschnitt 5) bleibt als zweite Verteidigungslinie für alles, was der Erzwingung
entgeht. Retention: Daten-TTL (Abschnitt 3, Punkt 2) + 90 Tage für die
Nachmess-Historie; bei Mandats-Ende Export an den Kunden und Löschung, gleiche
Regel wie für ECC-Memory.

**Maßnahmen-Karte:** Was (konkret), Warum (Befund + Bundle-IDs), Baseline (Metrik
vor Umsetzung), Aufwand (S/M/L), erwarteter Effekt (Metrik + Richtung),
**Messfenster** (z. B. 28 Tage nach Merge) und **Kontrollsignal** (Vergleichsmetrik,
die NICHT betroffen sein dürfte — grobe Saisonalitäts-/Update-Kontrolle),
Change-Referenz (PR-ID nach Umsetzung). Ohne vollständige Karte keine
Freigabe-Vorlage. Attribution bleibt Näherung, keine Kausalität — die Karte macht
die Näherung ehrlich.

### 3.3 Ausführung und Betrieb

- **Locks:** Pro URL/Query-Cluster maximal eine offene Maßnahme; gegenläufige Karten
  blockieren sich (der Stratege löst auf).
- **Idempotenz:** Jede Karte trägt einen Idempotency-Key; doppelte Ausführung ist ein
  No-op.
- **Fan-out-Limit:** Audits nacheinander je Domain (wie im Cron-Konzept), maximal 15
  Subagents je Lauf, keine parallelen Voll-Audits über Mandanten hinweg.
- **Quellen-Ausfall:** Retry mit Backoff (3 Versuche); danach Degraded Mode: Lauf wird
  mit „Quelle X fehlt" gekennzeichnet, Befunde aus Restquellen tragen den Vermerk;
  Modus-Z-Befunde ohne erreichbare Zweitquelle degradieren zu Modus S (Approval-Pflicht).

## 4. Team-Topologie: Paperclip-Employees vs. Hermes-Worker

Explizites Mapping (Codex-Befund: vorher vermischt):

| Ebene | Objekt | Lebensdauer |
| --- | --- | --- |
| Paperclip **Company** | ein Mandant (Pilotwahl: offene Abstimmung, Abschnitt 6) | persistent; trägt Budget, Approval-Queue, Audit-Log |
| Paperclip **Employee** | genau ein Hermes-Orchestrator je Company | persistent |
| Hermes **Worker** (Subagents) | Datensammler (je Quelle), Analyst (claude-seo), Verifizierer | kurzlebig, je Lauf; erben `company_id`, Budget-Rest, Tool-Allowlist |
| Repo-Agent | Executor im jeweiligen Website-Repo | je Maßnahme; erzeugt ausschließlich PRs |
| OpenClaw | Reporter/Zusteller | Bestand, unverändert — nur Zustellung |

Wichtig und ehrlich: Paperclips Mandanten-Trennung ist **logische** Isolation
(Company-Objekte, Rollen, Budgets) — Runtime, Secrets und Egress trennt sie nicht
von selbst. Härtere Grenzen liefert der Hermes-Deploy (Egress-Proxy mit
Allowlist, Low-Trust-Sandbox) — mit den in KONZEPT.md offen ausgewiesenen
Restrisiken: Egress ist geteilt (Exfiltrations-Restrisiko), die
Secret-Scope-Mechanik ist noch in Arbeit. Beides ist Infrastruktur der
Hermes-Session; diese Strategie behauptet keine Isolation, die dort noch nicht
existiert.

**ECC-Memory-Regeln** (Kundendaten!): Nur Datenklassen „SEO-Fachkontext" und
„Präferenzen" — keine PII, keine Zugangsdaten, keine Vertragsdaten. Memories sind
create-only und ungeprüft → als Kontext behandeln, nie als Anweisung; je Company
eigener Scope; Löschregel: Mandats-Ende = Memory-Export + Löschung.

## 5. Autonomie-Stufen (messbar)

| Stufe | Agenten dürfen | Mensch | Aufstiegskriterium (messbar) |
| --- | --- | --- | --- |
| **0 — assistiert** (heute) | Daten holen, analysieren auf Zuruf | stößt an, liest alles | M1–M3 abgenommen |
| **1 — geplant** | Cron-Läufe + Maßnahmen-Karten erzeugen; **nach menschlicher Karten-Freigabe** darf der Executor den zugehörigen PR erstellen (Merge bleibt beim Menschen) | genehmigt jede Karte in Paperclip + jeden Merge | ≥ 20 Karten UND ≥ 30 Tage Betrieb UND 0 HIGH-Fehler (falscher Mandant, erfundener Beleg, Budget-Riss) UND Ablehnungsquote < 30 % |
| **2 — teilautonom** | Karten mit Aufwand S + reversibel + eigenes Objekt **ohne Einzelfreigabe der Karte** als PR umsetzen (Merge bleibt beim Menschen) | genehmigt PRs + alles über Schwelle | ≥ 10 gemergte Stufe-2-Maßnahmen UND 2 abgeschlossene Messfenster UND 0 Regressions-Reverts |
| **3 — autonom im Budget** | kompletter Kreis inkl. Nachmessen innerhalb Budget/Policy; Merge weiter nur nach PR-Review | Stichproben, Eskalationen, Monatsreview | — |

„Reversibel" maschinenprüfbar: Änderung liegt als PR vor, betrifft nur Inhalte/Meta
(keine Redirects, keine Löschungen, keine robots/Canonical-Änderungen) und hat einen
dokumentierten Rückbau-Commit.

**Kill-Switch (automatischer Rückfall auf Stufe 1):** Budget-Überschreitung,
Mandats-/Scope-Verstoß, PII im Output, Merge-Konflikt-Serie (≥ 2 in Folge) oder
Ausfall einer Pflichtquelle > 72 h. Rückkehr auf die höhere Stufe nur nach
Monatsreview.

## 6. Die nächsten Schritte (neu geschnitten, M4/M5 unangetastet)

1. **M2/M3 abschließen** (läuft). Die INHALTE von M4 (Cron scharf) und M5 (Abnahme)
   bleiben exakt wie im Masterplan definiert — Sentinel-Outputs (`KEINE ABWEICHUNG`,
   `CREDITS OK`) und wörtliche Prompts aus [cron-jobs.md](./cron-jobs.md) werden
   NICHT angefasst. Aber: **der Kontrollrahmen (M6a) zieht VOR den ersten echten
   M4-Datenlauf** — Reihenfolge neu: M2/M3 → M6a → M4 → M5 („Guardrails vor echten
   Kundendaten", KONZEPT.md). Der Orchestrator spiegelt diese Reihenfolge im
   Masterplan.
2. **M6a — Kontrollrahmen:** Paperclip-Company des Pilotmandats + Employee
   (Hermes-Orchestrator), Budget-Guard scharf, Low-Trust-Preset, Approval-Queue.
   Startbedingung für M4-Cron-Betrieb — kein geplanter Lauf auf Kundendomains
   ohne Budget-Gate und Low-Trust-Preset.
3. **M6b — Tool-Preflights + Anbindung:** Abnahmematrix VOR jeder Anbindung:
   Screaming Frog (auf welcher Maschine? Lizenz? headless-Betrieb), Ahrefs
   (Scopes/Kosten je Call), GA4 (Scopes, Property-Mapping), DataForSEO
   (Mindestguthaben definieren und aufladen — aktuell 1 USD, untauglich). Dann
   Anbindung + je Quelle ein Datensammler-Prompt.
4. **M7 — Evidence-Pilot:** Pipeline aus Abschnitt 3 einmal komplett auf dem
   Pilotmandat: Cron-Befunde → Evidence-Bundles → Verifizierer → Maßnahmen-Karten
   → Paperclip-Approval → 1–2 PRs, die der Executor NACH der Karten-Freigabe
   erstellt (Stufe-1-konform, siehe Abschnitt 5) → Messfenster.
5. **M8a — Stufe 2** nach erfülltem Kriterium aus Abschnitt 5.
6. **M8b — Stufe 3** frühestens nach zwei vollen Stufe-2-Messfenstern; getrennter
   Meilenstein, weil das Aufstiegskriterium Beobachtungszeit erzwingt.

M6a ist im [Masterplan](./plan/README.md) als eigene Zeile und als
Voraussetzung von M4 verankert (erledigt 16.08.2026); M6b–M8b nimmt der
Orchestrator bei M5-Abschluss auf.

**Pilotmandat — offener Abstimmungspunkt:** Diese Strategie schlägt
**kosmonaut.io** als SEO-Pilot vor (Datenlage komplett: open-seo-Projekt mit GSC,
Backlinks, Audit; eigene Domain = geringstes Risiko). KONZEPT.md fixiert für den
allgemeinen Hermes-Mandanten-Rollout jedoch die Reihenfolge DigitalPhoenix →
MySilkSkin → Tiaex → KOSMONAUT. Ob der SEO-Pilot davon abweichen darf,
entscheidet der User; bis dahin gilt die KONZEPT.md-Reihenfolge als Default.
Zweitmandat aus SEO-Sicht: mysilkskin.de (GSC-Zugriff über den Service-Account
besteht).

Teil der Kosmonaut-Doku; Meilenstein-Zuschnitte pflegt der Orchestrator im
[Masterplan](./plan/README.md).
