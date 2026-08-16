# Council-Urteil — 60-Tage-Priorisierung kosmonaut.io

17.08.2026 · 4 Perspektiven-Agenten (SEO, CRO/Leads, Brand, Delivery) + Synthese
(Workflow `kosmonaut-content-council`, 5 Agenten). Eingaben:
[60-Tage-Plan](./pilot-kosmonaut-60-tage.md) ·
[Wettbewerb](./wettbewerb-kosmonaut.md) ·
[Karten Cluster A](./massnahmen/2026-08-16-cluster-a.md).
Dieses Urteil ÜBERSTIMMT bei Konflikt die Prioritäten des 60-Tage-Plans.


## 1. KONSENS

| Maßnahme | Stimmen (top3) | Befund |
|---|---|---|
| **Karte A1** (commercetools: Title/H1/Meta + interne Links) | **4/4** | Einstimmig. Klarster Hebel: 871 Impr./Pos 13/0 Klicks, kaputter Title, Aufwand S, reversibel. |
| **Karte A2** (OXID: H1-Intent + Title-Kürzung) | 3/4 (SEO, CRO, Delivery) | Gewinnbarste SERP (dixeno/netzfokus/kennersoft = Mini-Domains), Bestandsmarkt-Wert. |
| **Konfigurator-LP** (Cluster B) | 3/4 (CRO, Brand, Delivery) | Pos 2,4 ohne dedizierte Seite, höchste Lead-Qualität, differenzierendes eigenes Angebot, relaunch-sicher. |
| **Karte A3** (API-Artikel: kommerzieller Block) | 3/4 (CRO, Brand, Delivery) | Freigabe ja — aber mit SEO-Auflage (siehe Dissens 3). |
| **Cluster C verschieben** | **4/4** in streichen | Einstimmig: Pos 50–70 gegen CRO-Spezialisten, Erfolgskriterium liefert 0 Klicks/0 Leads im Fenster. |
| **Cluster D verschieben/eindampfen** | 3/4 in streichen | Ziel Pos 20–25 = außerhalb Klick-Zone, härtester Wettbewerb, Doppelarbeit zum Relaunch. |

## 2. DISSENS — Entscheidungen des Vorsitzenden

**D1 · „shopify plus agentur" (2.956 Vol, KD 1, Pos 41)**
- *Pro (SEO + CRO):* Größter unbearbeiteter Hebel, weichste SERP, bestes Volumen/Aufwand-Verhältnis — Fehlpriorisierung, dass die Karte fehlt.
- *Contra (Brand):* Fünfte Plattform-Fahne = Bauchladen-Signal; SERP gehört Pure-Playern mit Case-Substanz, KOSMONAUT hat keine sichtbaren Shopify-Plus-Referenzen. „Erst Cases, dann Keyword."
- **Entscheidung:** Karte JA, aber konditional (2:1-Votum + Zahlenlage schlagen das Positionierungsargument nicht weg, sie relativieren es). Vorbedingung: Referenz-Check — existiert mindestens ein belastbarer Shopify-/Shopify-Plus-Case? **Ja** → Karte in Woche 4–5, bevorzugt als Schärfung der bereits auf Pos 41 rankenden URL (kein dritter Neubau; Delivery-Limit: eine neue Seite = Konfigurator). **Nein** → Beobachtung, Keyword wandert ins Relaunch-Briefing. Damit ist auch Brands Kernsorge (Claim ohne Substanz) adressiert.

**D2 · Cluster F / Headless-Agentic-Hub**
- *Pro (Brand):* Einziges Feld, wo KOSMONAUT First Mover ist; Zeitfenster schließt sich; das IST die Positionierung.
- *Contra (Delivery + CRO):* Ein Hub ist Informationsarchitektur → Relaunch-Scope; jetzt auf alter IA gebaut = Wegwerf-Arbeit; keine Leads in 60 Tagen; KD 0 heißt auch: das Feld läuft nicht weg.
- **Entscheidung:** Delivery gewinnt für das 60-Tage-Fenster — Hub in den Relaunch-Scope, Tracking der 17 Keywords läuft als Baseline weiter. Brands Punkt wird aber nicht vertagt, sondern verkleinert mitgenommen: (a) A1 framt commercetools explizit als Composable/Headless-Kompetenz, (b) das Positionierungs-Statement (ein Satz) wird im Pilot verabschiedet, damit der Relaunch-Hub nicht bei null startet. Einzig „headless commerce agentur" (168, kommerziell) bleibt im aktiven Tracker.

**D3 · Karte A3 — sofort vs. Kannibalisierungs-Stopp**
- *Pro (CRO/Brand):* Stärkste Einzelkarte, kürzester Weg von Impression zu Anfrage.
- *Contra (SEO):* Eingebaute Kannibalisierung — Artikel jetzt kommerziell optimieren, später Expertise-Seite auf dieselbe Query; Title-Anhang gefährdet das Kontrollsignal „api-schnittstellen" (2.400 Vol).
- **Entscheidung:** Beides stimmt, der Konflikt ist auflösbar. A3 wird freigegeben **ohne** den Title-Anhang „| KOSMONAUT API-Agentur" (H2-Block + Referenzen + CTA ja). Gleichzeitig wird jetzt entschieden und in die Keyword→URL-Matrix geschrieben: **Der Artikel IST die dauerhafte Ranking-URL für `api schnittstellen agentur`.** Eine spätere Expertise-Seite kommt nur mit 301-/Konsolidierungsplan — nicht daneben.

**D4 · Cluster E / MDM-Refresh**
- *Pro (Delivery):* Bestes Aufwand/Wirkung-Verhältnis nach A, füttert die A-Seiten per interner Links.
- *Contra (CRO):* 8.948 Impressionen auf reinem Info-Intent = Traffic-Eitelkeit; Ahrefs/GSC-Widerspruch (Pos 1 vs. 35) ungelöst.
- **Entscheidung:** Split. /checklisteonlineshop/ → Refresh + Snippet + Links (Konsens-fähig, näher am kommerziellen Intent). /mdm/ → NUR interne Links auf kommerzielle Seiten; Refresh erst nach Klärung des Datenwiderspruchs (GSC gilt als Wahrheit). Zusatzauflage aus SEO-Votum: das Mapping „agentur e-commerce strategie → /checklisteonlineshop/" wird aufgelöst — die Seite ist Linkgeber-Asset, keine kommerzielle Ziel-URL.

## 3. FINALE PRIORITÄTENLISTE (60 Tage)

| # | Maßnahme | Woche | Warum |
|---|---|---|---|
| 1 | **Blocker-Task Tag 1:** Strapi-Content-Model-Audit (SEO-Felder je Seite?), Entscheidung Repo vs. CMS, Deploy-/Revalidierungs-Pfad — mit Owner und Datum | 1 | Ohne das ist jede „Aufwand: S"-Schätzung Fiktion; hängt als Fußnote unter allen A-Karten (3 Perspektiven nennen es als Top-Risiko). |
| 2 | **Karte A0: Conversion-Audit + Lead-Messkette** — Live-Check aller Ziel-URLs auf Formular/CTA/Trust, GA4-Events (Formular, CTA-Klick), Anfragen-Baseline 90 Tage, Definition „qualifizierte Anfrage" | 1 | Ohne sie ist Tag 60 nicht auswertbar — das Mandat heißt Leads, nicht Klicks. Aufwand S. |
| 3 | **DataForSEO aufladen (~20 $) + Indexierungs-Baseline** (GSC-Abdeckung /expertise/ + /projekte/, Sitemap-Status, interne Linktiefe) | 1 | Ohne Aufladung endet das Tracking nach Woche 1; 12–13 Keywords im Index bei DR 33 kann ein strukturelles Problem sein, das kein Title-Fix löst. |
| 4 | **Karte A1** mergen — erweitert um Proof-Block (Award, Konzern-Logos, 2 Case-Teaser), CTA above the fold, Composable/Headless-Framing | 1 | 4/4-Konsens; jede Woche Verzug kostet ein Sechstel des Messfensters. |
| 5 | **Karte A2** mergen — gleiche Proof-/CTA-Erweiterung | 1–2 | Gewinnbarste SERP im Set; Bestandsmarkt. |
| 6 | **Karte A3** mergen — ohne Title-Anhang, mit H2-Block + CTA; Ranking-URL-Entscheidung dokumentiert | 2 | Pos 9,1 → kürzester Weg zur ersten Anfrage. |
| 7 | **Keyword→URL-Matrix** verabschieden: genau eine Ziel-URL je Fokus-Query + Konsolidierungsregeln (Konfigurator-Streuseiten, API-Artikel) | 2 | Vorbedingung für jeden Neubau; verhindert die zwei bereits eingeplanten Kannibalisierungen. |
| 8 | **Cluster E (Teil 1):** /checklisteonlineshop/ Refresh + Snippet + interne Links (u. a. „OXID Agentur"-Anker); /mdm/ nur Links | 2–3 | Doppelter Hebel pro Stunde, füttert Cluster A, relaunch-sicher. |
| 9 | **Konfigurator-LP** (Cluster B): content-first in Strapi, dediziertes Formular „Konfigurator-Projekt anfragen", Cases, Konsolidierung der Streuseiten gemäß Matrix — **live bis Tag 30** | 3–4 | 3/4-Konsens; einzige Neue-Seite-Investition, die sich im Fenster rechnet; höchste Lead-Qualität. |
| 10 | **Karte Shopify Plus** (konditional, s. D1): Referenz-Check, dann On-Page-Schärfung der rankenden URL auf Agentur-Intent | 4–5 | 2.956 Vol / KD 1 / Pos 41 — nur wenn Cases existieren und A+B bis Tag 30 gemerged sind. |

**Harte Regel für alles:** Merge-Deadline Tag ~25 — was später merged, liefert am Tag 60 keine belastbaren Zahlen (Crawl-Latenz bei 34 Besuchern/Monat: 1–3 Wochen). Nach jedem Merge: GSC „Indexierung beantragen" + Verifikation im gerenderten HTML.

## 4. STREICH-/VERSCHIEBE-LISTE

| Was | Verdikt | Begründung |
|---|---|---|
| **Cluster C** (CRO-Leistungsseite) | Verschieben auf Tag 60+ / Relaunch-Content; als 6–12-Monats-Wette deklarieren | 4/4. Pos 50–70, Spezialisten-SERP, 0 Klicks im Fenster, off-brand ohne Beweis-Assets; unrealistisches Erfolgskriterium gefährdet die Report-Glaubwürdigkeit. |
| **Cluster D** (Shopware-Relaunch) | Eindampfen auf Content-only (Cases, Partner-Beleg, interne Links); Struktur-Arbeit → Relaunch | Ziel liegt außerhalb der Klick-Zone; „Seiten-Relaunch" parallel zum Site-Relaunch = Doppelarbeit. |
| **Cluster F Hub** (Headless/Agentic/Composable) | Relaunch-Scope; Tracking bleibt; Positionierungs-Statement im Pilot | Siehe D2. |
| **MDM-Refresh** | Halt bis Ahrefs/GSC-Widerspruch geklärt; nur interne Links | Auf widersprüchlicher Evidenz wird nicht gebaut. |
| **„findologic"** aus Cluster A | Streichen (nur Tracker) | Navigationale Fremdmarken-Query, keine Ziel-URL, Klick-Erwartung ~0. |
| **„headless cms" (1.812 Vol)** | Nur beobachten | Vendor-SERP, Intent-Mismatch; kommerziell zählt nur „headless commerce agentur". |
| **Mapping „agentur e-commerce strategie" → Checkliste** | Auflösen | Eine URL mit drei Jobs; Info-Seite gewinnt keine kommerzielle Query. |
| **DEPT-/partner/-Seitenebene** | Ausschließlich Relaunch-Scope | Neue URL-Ebene jetzt = doppelte Migration; als Anforderung ins Relaunch-Briefing. |
| **Tag-60-Erfolgskriterium Cluster C** („indexiert + Top 20") | Aus dem Pilot-Report streichen | Unerfüllbar; ersetzt durch Lead-KPIs aus A0. |

## 5. TOP-3-RISIKEN mit Gegenmaßnahme

1. **Strapi/CMS-Governance blockiert den gesamten Karten-Fluss** (von 3/4 Perspektiven genannt). Wenn Title/Meta/H1 nicht pflegbar sind, wird jede S-Karte zur Content-Model-Migration und alle Messfenster kippen. → **Gegenmaßnahme:** Priorität #1, Tag 1, mit Owner, Datum und Eskalationspfad; keine Karten-Freigabe vor Klärung. Zusätzlich WIP-Limit (max. 3 Karten in flight) und Freigabe-SLA 48 h, damit Stufe 1 nicht zum Flaschenhals wird.

2. **Der Pilot misst Traffic, das Mandat heißt Leads.** Kein Erfolgskriterium enthält „Anfrage"; ohne Baseline und Events ist am Tag 60 nicht feststellbar, ob eine einzige Anfrage aus den Clustern kam — inklusive Gefahr der Fehlentscheidung „SEO bringt keine Leads" wegen B2B-Latenz (2–6 Wochen Klick→Anfrage). → **Gegenmaßnahme:** Karte A0 vor allen On-Page-Karten; Zwischenmetriken (CTA-Klicks, Formular-Starts, Kontaktseiten-Aufrufe) als offizielle Tag-60-KPIs neben Rankings; Lead-Bewertung explizit erst Tag 90 als Nachmessung ansetzen.

3. **Eingeplante Kannibalisierung ohne Konsolidierungsregeln** (A3-Folge-Seite, Konfigurator-LP neben Streuseiten mit Pos 2,4–8,9). Im schlimmsten Fall zerstört die neue LP das beste Bestandsranking mitten im Messfenster — ein Netto-Verlust, den der Report KOSMONAUT selbst zuschreiben würde. → **Gegenmaßnahme:** Keyword→URL-Matrix (Priorität #7) ist harte Vorbedingung für jeden Neubau; je Query genau eine Ziel-URL plus definierte Regel (301 / Canonical / De-Optimierung) für jede verdrängte Seite; Kontrollsignale der bestehenden Rankings in den wöchentlichen Rank-Check aufnehmen.

---
*Grundsatz des Urteils: Die vier Perspektiven widersprechen sich weniger, als es scheint — sie beschreiben dieselbe Lücke von vier Seiten: Der Plan ist ein guter Ranking-Plan, dem Vorbedingungen (CMS-Governance), Messkette (Leads) und Leitplanken (URL-Matrix) fehlen. Das Urteil ergänzt diese drei und schneidet alles ab, was im 60-Tage-Fenster keine Klicks oder Anfragen liefern kann.*
