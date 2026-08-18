# Indexierungs-Baseline kosmonaut.io — 18.08.2026

Vorbedingung 1d des Content-Plans (Fassung 2) · SEO-Session · Quellen:
GSC Service-Account (Sitemaps-API + Search Analytics 90 Tage, page-Dimension),
Sitemap-Abruf live (HTTP 200).

## Kernzahlen

| | |
| --- | --- |
| Sitemap `sitemap.xml` | **97 URLs**, 0 Fehler/Warnungen — zuletzt eingereicht **29.11.2025** (9 Monate alt) |
| Seiten mit Impressionen (90 d) | **80** |
| Dunkle Seiten (in Sitemap, 0 Impressionen) | **18** |
| Sichtbarkeit je Sektion | newsroom 35 Seiten/28.037 Impr · expertise 16/12.732 · projekte 11/3.338 · services 8/2.747 · Startseite 4.946 |

## Befund 1 — die Services-Ebene ist fast komplett dunkel

11 der 18 dunklen Seiten liegen unter `/services/`:
`a-b-testing-and-experimentation`, `analytics-insights`, `cx-audit-strategy`,
`designsystem-guidelines`, `digitale-strategie`, `loyalty-membership`,
`marketing-automation`, `personalization-recommendation`, `service-design`,
`technical-content-seo`, `usability-lab` — alle 0 Impressionen in 90 Tagen.

Relevanz: Das sind größtenteils CRO-/UX-nahe Leistungsseiten. Für den
CRO-Relaunch-Scope (Backlog) existiert also deutlich mehr Bestand als die 8
sichtbaren Services-Seiten — Konsolidierung gehört ins Relaunch-Briefing,
nicht Neubau. Restliche dunkle Seiten: `/expertise/website/` (deckt sich mit
D6-Entscheidung: 301 auf /services/), 4 alte News, 2 Projekt-Cases
(badheilbrunner, ip44).

## Befund 2 — dev-Subdomain ist teilindexiert (Hygiene/Leak)

`https://dev.kosmonaut.io/rechtlichehinweise/impressum/` sammelt Impressionen
in der GSC (sc-domain-Property fängt Subdomains). **Diagnose (KMT, 18.08.):
Konfiguration ist korrekt** — dev liefert robots.txt `Disallow: /` UND meta
`noindex,nofollow`. Es ist das klassische Robots-Paradox: Disallow verhindert
das Crawlen, dadurch liest Google das noindex nie („Indexiert, obwohl durch
robots.txt blockiert"). Abhilfe: (a) GSC-Removal-Request für die
dev-Subdomain — das Removals-Tool ist UI-only (kein API-Endpoint), also ein
Anatolij-Klick, auf der gebündelten Entscheidungsliste; (b) Disallow auf
Nicht-Prod lockern, damit noindex gelesen wird — Backlog, kein
Offensive-Blocker.

## Befund 3 — Kleinigkeiten

- Doppel-Slash-URL `https://kosmonaut.io//` mit 11 Impressionen (Duplikat der
  Startseite) — Canonical/Redirect-Hygiene im Relaunch.
- Sitemap seit 29.11.2025 nicht neu eingereicht; nach den ersten
  Karten-Merges (Regel 7: „Indexierung beantragen") auch die Sitemap neu
  einreichen.

Teil der Kosmonaut-Doku · [Matrix](./keyword-url-matrix.md) ·
[Pilot](./pilot-kosmonaut-60-tage.md)
