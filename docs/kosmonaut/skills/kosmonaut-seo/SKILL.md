---
name: kosmonaut-seo
description: "Einstiegspunkt für alle SEO-Aufgaben bei Kosmonaut. Entscheidet, welches Werkzeug eine Anfrage bearbeitet: claude-seo für alles am eigenen Objekt (On-Page, Technik, Schema, Core Web Vitals, Bilder, hreflang, GSC/GA4/CrUX, Content-Brief, Cluster, GEO, SXO), open-seo für Keyword-Daten, Rankings über Zeit und fremde Domains, ECC für Text und Marke. Verhindert doppelte Arbeit und unnötige DataForSEO-Kosten. Triggert auf: SEO, Audit, Keywords, Rankings, Backlinks, Wettbewerber, Sichtbarkeit, Content-Brief, technisches SEO."
user-invocable: true
argument-hint: "[aufgabe] [domain]"
license: MIT
metadata:
  author: Kosmonaut
  version: "1.0.0"
  category: seo
---

# Kosmonaut SEO — Router

Einziger Einstiegspunkt für SEO-Arbeit. Diese Datei entscheidet **nicht**, wie SEO geht —
das können die Fachskills besser. Sie entscheidet, **wer** eine Anfrage bearbeitet.

## Leitsatz

**Kostenlos vor kostenpflichtig. Eigenes Objekt vor Fremddaten. Flüchtig vor persistent.**

claude-seo arbeitet direkt am Live-Objekt und an Googles eigenen APIs — das kostet nichts.
open-seo kauft Daten bei DataForSEO. Rufe open-seo erst, wenn eine Entscheidung Historie
oder Daten über eine fremde Domain braucht, nie als Standardeinstieg.

## Routing

| Anfrage dreht sich um                          | Nimm       | Konkret                                            |
| ---------------------------------------------- | ---------- | -------------------------------------------------- |
| On-Page, Technik, Crawlability, Indexierung    | claude-seo | `/seo technical`, `/seo page`                      |
| Schema, strukturierte Daten                    | claude-seo | `/seo schema`                                      |
| Core Web Vitals, Ladezeit                      | claude-seo | `/seo google`, `/seo technical`                    |
| Bilder                                         | claude-seo | `/seo images`                                      |
| Sitemap, hreflang, Internationalisierung       | claude-seo | `/seo sitemap`, `/seo hreflang`                    |
| GSC-, GA4-, CrUX-Daten **eigener** Properties  | claude-seo | `/seo google`                                      |
| Vollständiges Site-Audit                       | claude-seo | `/seo audit` — delegiert selbst parallel           |
| Content-Qualität, E-E-A-T                      | claude-seo | `/seo content`                                     |
| Content-Brief                                  | claude-seo | `/seo content-brief`                               |
| Themencluster aus einem Seed                   | claude-seo | `/seo cluster`                                     |
| AI-Suche, AI Overviews, GEO                    | claude-seo | `/seo geo`                                         |
| Search Experience, Seitentypen, Personas       | claude-seo | `/seo sxo`                                         |
| Lokales SEO, GBP, Map Pack                     | claude-seo | `/seo local`, `/seo maps`                          |
| Veränderung gegenüber letztem Stand            | claude-seo | `/seo drift compare`                               |
| Backlinks, **erste Einschätzung**              | claude-seo | `/seo backlinks` — Moz, Bing, Common Crawl         |
| Keyword-Volumen, Difficulty, SERP-Zeilen       | open-seo   | `research_keywords`, `get_keyword_metrics`         |
| Rankings **über Zeit**                         | open-seo   | Rank-Tracker                                       |
| Fremde Domain, Wettbewerber-Footprint          | open-seo   | `get_domain_overview`, `get_ranked_keywords`       |
| Backlinks, **belastbar für eine Entscheidung** | open-seo   | `get_backlinks_profile`                            |
| Text schreiben, Markenstimme, Repurposing      | ECC        | `article-writing`, `brand-voice`, `content-engine` |
| Marktrecherche, Wettbewerbsbericht             | ECC        | `market-research`, `competitive-report-structure`  |
| Kundenkontext, frühere Entscheidungen          | ECC        | Memory Vault, `.ecc/memory/`                       |

### Grenzfälle

**„Wie stehen wir bei Keyword X?"** — Eigene Property: claude-seo über GSC, das sind echte
Zahlen statt geschätzter. Fremde Domain: open-seo.

**„Analysiere Wettbewerber Y."** — open-seo für den Footprint (Keywords, Traffic, Backlinks),
danach claude-seo für die konkrete Seite, wenn es um Machart statt Volumen geht.

**„Vollständiges Audit für Kunde Z."** — `/seo audit`. Nicht vorher open-seo abfragen; das
Audit sagt selbst, wo Marktdaten die nächste Entscheidung tragen würden.

**„Backlinks von Domain A."** — Erst claude-seo. Reicht das Ergebnis für die anstehende
Entscheidung, ist Schluss. Nur wenn eine Investition davon abhängt, open-seo nachlegen.

## Vor jedem open-seo-Aufruf

1. `whoami` — bestätigt Verbindung und zeigt verbleibende Credits.
2. `list_projects`, um die `projectId` aufzulösen; existiert kein Projekt für die Domain,
   `create_project`.
3. Bei größeren Abfragen vorher sagen, was sie ungefähr kostet, und bestätigen lassen.

## Fehlt eine autorisierte Datenquelle: nichts beschaffen

Gilt für **jede** Quelle dieser Tabelle — open-seo nicht verbunden, claude-seo-Runtime nicht
vorhanden, Credentials fehlen, Kostenfreigabe verweigert.

**Dann gilt ausnahmslos:** benennen, welche Frage unbeantwortet bleibt, und welche Quelle
dafür fehlt. Danach ist Schluss.

**Nicht erlaubt** — auch nicht, wenn es technisch ginge, und auch nicht „nur zur
Orientierung":

- keine Suchmaschine im Browser aufrufen, um SERP-Zeilen abzulesen
- kein `curl`, `wget` oder Skript gegen Suchmaschinen, Aggregatoren oder fremde SEO-Tools
- **keine gefälschten User-Agents** — nie einen Browser vortäuschen, den es nicht gibt
- kein Ausweichen auf eine andere Datenquelle, ohne dass der Wechsel in der Antwort steht
- keine Schätzwerte, keine aus dem Gedächtnis ergänzten Zahlen

Der Grund ist nicht Vorsicht, sondern die Zusage des Produkts: Jede Zahl in einem
Kosmonaut-Report trägt Quelle und Konfidenz. Eine stillschweigend getauschte Quelle bricht
genau diese Zusage — und der Kunde merkt es erst, wenn er die Zahl verteidigen muss.

Ist eine Ersatzquelle fachlich sinnvoll, wird sie **vorgeschlagen, nicht verwendet**:
„Für X fehlt open-seo. Möglich wäre Y, mit Einschränkung Z — soll ich?"

*Anlass: Werkbank-Akzeptanz M2 (Hermes-Lead, 19.08.2026). Ein Testlauf traf die Zeile
„Keyword-Volumen, Difficulty, SERP-Zeilen → open-seo" bei nicht verbundenem open-seo. Die
Verfügbarkeitsprüfung lief korrekt; danach wich der Agent selbstständig auf Browser-Suche
und dann auf `curl` mit gefälschtem Chrome-User-Agent gegen html.duckduckgo.com aus und
extrahierte SERP-Zeilen aus dem gescrapten HTML. Der alte Wortlaut verbot nur Schätzwerte —
die Beschaffung selbst war nicht ausgeschlossen.*

## Namenskollisionen

Fünf Skills triggern auf „SEO". Der Router ist der einzige Einstieg; die anderen werden von
ihm gerufen oder liegen in einer anderen Runtime.

| Skill                                | Runtime     | Regel                                                                                 |
| ------------------------------------ | ----------- | ------------------------------------------------------------------------------------- |
| `claude-seo` (`/seo …`)              | Hermes      | vom Router gerufen                                                                    |
| `open-seo` (`seo-audit`)             | Hermes      | vom Router gerufen, nur nach Kostenprüfung                                            |
| `ECC` (`seo` aus `business-content`) | Hermes      | **nicht verwenden** — Beifang des Marketing-Moduls, fachlich von claude-seo abgedeckt |
| `searchfit-seo:seo-audit`            | Claude Code | Bestand, andere Runtime                                                               |
| `brightdata-plugin:seo-audit`        | Claude Code | Bestand, andere Runtime                                                               |

## Was dieser Router nicht kann

**Über die Runtime-Grenze rufen.** Die Bestandsskills — `weekly-seo-report` mit ahrefs,
Sistrix, ClickUp und Notion — laufen in Claude Code, nicht in Hermes. Ein Auftrag, der
Analyse und Wochenbericht verbindet, braucht zwei Aufrufe in zwei Runtimes.

Kommt eine Anfrage nach Wochenbericht, ClickUp-Tasks oder Notion-Ablage: nicht selbst
nachbauen. Sagen, dass das in Claude Code über `weekly-seo-report` läuft, und die
Analyseergebnisse so aufbereiten, dass sie dort direkt weiterverwendbar sind.

## Nach jeder Analyse

Reicht das Ergebnis für eine Entscheidung, PDF anbieten: `/seo google report`.
Ergibt sich neuer Kundenkontext — Positionierung, Einschränkungen, getroffene
Entscheidungen — in den Memory Vault schreiben, nicht nur in die Antwort.
