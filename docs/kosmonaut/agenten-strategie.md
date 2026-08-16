# Von Tools zu Entscheidungen: Agenten-Teams für Kosmonaut-SEO

Strategie-Ergänzung zum [Konzept](./seo-team-hermes.md) und [Masterplan](./plan/README.md).
Stand 16.08.2026. Die Technik (M1 fertig, M2 delegiert, M3 vorbereitet) beantwortet
„womit". Dieses Dokument beantwortet „wer entscheidet was, auf welcher Datenbasis,
mit wie viel Autonomie".

## 1. Das Ziel in einem Satz

Eigenständige Agenten-Teams in Hermes (orchestriert über Paperclip) holen Daten aus
allen angebundenen Quellen, analysieren und **verifizieren** sie gegeneinander, und
leiten daraus Marketing- und SEO-Maßnahmen ab — der Mensch genehmigt Maßnahmen,
er produziert sie nicht.

## 2. Datenquellen-Matrix (Ist-Stand 16.08.2026)

| Quelle | Daten | Zugang | Status |
| --- | --- | --- | --- |
| open-seo (selfhost) | Keywords, SERP, Rankings über Zeit, Backlinks, Audit-Historie, DataForSEO | MCP `https://openseo.tiaex.ai/mcp` + 7 Skills (`~/.claude/skills/`) | **live**, M3 verdrahtet es in Hermes |
| Google Search Console | Queries, Klicks, Impressionen, Positionen, Indexierung | (a) Service-Account via claude-seo (`gsc_query.py`, 4 Properties), (b) open-seo-Connect (kosmonaut.io verbunden) | **live** |
| claude-seo | On-Page, Technik, Schema, CWV, GEO, Local, Drift — 25 Skills, 18 Agents, 53 Scripts | Skills direkt in Hermes (M2) | M2 läuft |
| Screaming Frog v24+ | Voll-Crawls, Bulk-Exports, URL-Inspektion, Screenshots | **nativer MCP-Server** (~29 Tools, headless-fähig) | anzubinden (M6) |
| Ahrefs | Backlinks, organische Keywords, Content | offizieller Ahrefs-MCP (claude-seo-Extension `/seo ahrefs`) | Zugang vorhanden, in Hermes anzubinden (M6) |
| Sistrix | Sichtbarkeitsindex, Wettbewerb | Bestand `weekly-seo-report` (Claude Code); Sistrix-API für Hermes prüfen | Bestand läuft |
| Google Analytics 4 | Traffic, Conversions, Verhalten | open-seo-Connect (gleicher OAuth-Client, `/api/ga4/oauth/callback`) + claude-seo `ga4_report.py` | vorbereitet, bewusst später |

Regel bleibt die Routing-Tabelle aus dem Konzept: kostenlos vor kostenpflichtig,
eigenes Objekt vor Fremddaten, flüchtig vor persistent.

## 3. Die Entscheidungs-Pipeline (der Kern)

Jede wiederkehrende SEO-Arbeit läuft als geschlossener Kreis:

```
HOLEN → ANALYSIEREN → VERIFIZIEREN → MASSNAHME ABLEITEN → FREIGEBEN → AUSFÜHREN → MESSEN
  │         │              │                │                │           │          │
  │         │              │                │                │           │          └─ GSC/GA4/open-seo-Historie
  │         │              │                │                │           └─ Agent im Website-Repo (PR, nie direkt live)
  │         │              │                │                └─ Paperclip Approval-Queue (Mensch)
  │         │              │                └─ Maßnahmen-Karte: Was, Warum, Beleg, Aufwand, erwarteter Effekt
  │         │              └─ ZWEITQUELLE bestätigt Befund (siehe 3.1)
  │         └─ claude-seo-Skills (Methodik) auf den Rohdaten
  └─ je Quelle ein Datensammler-Agent (MCP/Script)
```

### 3.1 Verifizieren heißt Cross-Source

Kein Befund wird zur Maßnahme, den nur eine Quelle behauptet:

| Befund-Typ | Primärquelle | Pflicht-Gegencheck |
| --- | --- | --- |
| Ranking-Verlust | open-seo Rank-Tracker | GSC-Positionen derselben Query |
| Backlink-Chance/-Risiko | Ahrefs | open-seo-Backlinks oder Moz/CommonCrawl (claude-seo) |
| Technik-Problem | Screaming-Frog-Crawl | claude-seo `/seo technical` am Live-Objekt |
| Content-Lücke | open-seo Keyword-/SERP-Daten | GSC Striking-Distance (Position 5–20) |
| Traffic-Anomalie | GA4 | GSC-Klicks + open-seo-Drift-Baseline |

Widersprechen sich Quellen, eskaliert der Verifizierer an den Menschen — mit beiden
Datenständen, nicht mit einer Meinung.

### 3.2 Maßnahmen-Karte als Übergabeformat

Jede abgeleitete Maßnahme hat: Was (konkret), Warum (Befund + Beleg-Links),
Datenbasis (Quellen + Datum), Aufwand (S/M/L), erwarteter Effekt (Metrik + Richtung),
Messpunkt (wann wird nachgemessen). Ohne vollständige Karte keine Freigabe-Vorlage.

## 4. Team-Topologie in Hermes/Paperclip

| Rolle | Träger | Aufgabe |
| --- | --- | --- |
| Orchestrator | Hermes-Hauptagent + `kosmonaut-seo`-Router | zerlegt Auftrag, wählt Quellen nach Routing-Tabelle |
| Datensammler (je Quelle) | Hermes-Subagents mit je einem MCP/Skill | holen Rohdaten, keine Interpretation |
| Analyst | claude-seo-Skills/-Agents | wendet Methodik an, erzeugt Befunde |
| Verifizierer | eigener Subagent, frischer Kontext | Cross-Source-Check nach 3.1, adversarial |
| Stratege | Hermes-Agent mit ECC-Memory (Kundenkontext) | priorisiert Befunde zu Maßnahmen-Karten |
| Executor | Agent im jeweiligen Website-Repo | setzt freigegebene Maßnahmen als PR um |
| Reporter | OpenClaw | stellt zu: Approvals, Alerts, Wochenbericht |

Paperclip liefert dazu Approval-Queue, Budget-Guard (DataForSEO-/API-Kosten!),
Audit-Log und Mandanten-Trennung (Company je Kunde) — nichts davon bauen wir selbst.

## 5. Autonomie-Stufen (der Weg dorthin)

| Stufe | Was Agenten dürfen | Mensch | Voraussetzung |
| --- | --- | --- | --- |
| **0 — assistiert** (heute) | Daten holen, analysieren auf Zuruf | stößt an, liest alles | M1–M3 |
| **1 — geplant** | Cron-Läufe (Drift, Audit, Credits) + Maßnahmen-Karten erzeugen | genehmigt jede Maßnahme in Paperclip | M4 + Pipeline aus 3 |
| **2 — teilautonom** | Maßnahmen unter Schwellwert (S-Aufwand, reversibel, eigenes Objekt) selbst als PR umsetzen | genehmigt PRs + alles über Schwellwert | 1 Monat Stufe-1-Historie ohne Fehlgriffe |
| **3 — autonom im Budget** | kompletter Kreis inkl. Nachmessen, innerhalb Budget/Policy | Stichproben, Eskalationen, Monatsreview | belastbare Messhistorie aus Stufe 2 |

Nie überspringen: Jede Stufe erzeugt die Beleg-Historie, die die nächste rechtfertigt.

## 6. Die nächsten Schritte (konkret)

1. **M2/M3 abschließen** (läuft): Hermes-Werkbank + MCP-Verbindung. Erst dann existiert
   der Ort, an dem Teams arbeiten.
2. **M4 erweitern**: Die drei Cron-Läufe aus [cron-jobs.md](./cron-jobs.md) sind
   Stufe-1-Prototypen — ihre Outputs auf das Maßnahmen-Karten-Format umstellen.
3. **M6 — Datenquellen-Vollausbau** (neu, nach M5): Screaming-Frog-MCP (nativ, v24)
   und Ahrefs-MCP in Hermes registrieren; je Quelle einen Datensammler-Prompt;
   Sistrix-API-Anbindung prüfen (sonst bleibt sie im Bestand). GA4-Connect in
   open-seo (vorbereitet). DataForSEO-Guthaben aufstocken.
4. **M7 — Verifizierer + Maßnahmen-Loop** (neu): Cross-Source-Regeln aus 3.1 als
   Verifizierer-Agent; Paperclip-Approval-Queue anbinden; erster Executor im
   kosmonaut-Website-Repo (nur PRs).
5. **M8 — Autonomie-Stufen 2→3**: Schwellwert-Policy in Paperclip, Budget-Guard
   scharf, Monatsreview-Ritual.

Pilotmandat für alles: **kosmonaut.io** (Projekt „KOSMONAUT Germany" in open-seo läuft
bereits mit GSC, Backlinks, Audit). Zweitmandat danach: mysilkskin.de (GSC-Zugriff
über den Service-Account besteht schon).

Teil der Kosmonaut-Doku; Änderungen an Meilenstein-Zuschnitten pflegt der
Orchestrator im [Masterplan](./plan/README.md).
