# Keyword→URL-Matrix kosmonaut.io — Erstfassung

Vorbedingung 1b des Content-Plans (Fassung 2, kmt-website-fix `980b784`) ·
Owner: SEO-Session · Stand 17.08.2026 · fällig 24.08.2026.
Datenbasis: GSC Service-Account, 90 Tage, Dimensionen query+page (Evidenzregel:
entscheidungstragende Zahlen immer nach Query UND Seite aufgeschlüsselt).

**Regeln:** Je Fokus-Query genau eine Ziel-URL. Jede konkurrierende Seite bekommt
eine definierte Regel: 301, Canonical, De-Optimierung oder Zubringer (interner
Link mit hartem Anker auf die Ziel-URL, keine eigene Optimierung auf die Query).
Alle URLs mit abschließendem Schrägstrich (`trailingSlash: true`).

## 1. Fenster-Maßnahmen (Wochen 1–8)

| Fokus-Query | Ziel-URL | Konkurrierende URL (GSC 90 d) | Regel |
| --- | --- | --- | --- |
| commercetools agentur | `/expertise/commercetools/` (432 Impr. · Pos 15,3) | `/newsroom/news/commercetools/` — 472 Impr. · Pos 13,8 (rankt BESSER als das Ziel) | Zubringer: Anker „commercetools Agentur" auf die Expertise-Seite; keine eigene Optimierung. Beobachten — fällt das Ziel nach A1 nicht vor die News, Canonical prüfen |
| commercetools agentur (Zweitfund) | — | Startseite `/` — 174 Impr. · **Pos 3,6** | Keine Aktion (Brand-Homepage rankt natürlich); Kontrollsignal im Tracker |
| oxid agentur | `/expertise/oxid-shop/` (296 Impr. · Pos 14,9) | `/newsroom/news/oxid-zertifizierung/` — 4 Impr. · Pos 31,8 | Zubringer, Anker „OXID Agentur" |
| oxid partner | `/expertise/oxid-shop/` (21 Impr. · Pos 21,2) | — | Kontrollsignal Karte A2 |
| api schnittstellen agentur | `/newsroom/insights/apisunverzichtbar/` (152 Impr. · Pos 7,2) — **dauerhafte Ranking-URL, Council D3** | `/expertise/api/` — 100 Impr. · Pos 34,8 | Bestehende Expertise-Seite ist Relaunch-Konsolidierungsziel; im Fenster KEINE Optimierung dort auf diese Query. A3-Block verlinkt auf Kontaktseite, nicht auf `/expertise/api/` |
| shopware agentur | `/expertise/shopware-agentur/` (50 Impr. · Pos 46,4) | — | Content-only-Karte; kein Klick-Versprechen Tag 60 |
| shopify plus agentur | `/expertise/shopify-agentur/` (912 Impr. · Pos 35,7) | — | Karte startet W4–5 (Case + Partnerstatus bestätigt 17.08.) |
| agentur plentymarkets | `/expertise/plentymarkets/` (86 Impr. · Pos 17,7) | — | Kleine Intent-Karte |
| augmented reality konfigurator · ar produktkonfigurator · 3d konfigurator agentur · agentur 3d produktkonfiguratoren onlineshops · 3d e-commerce konfigurator | **Neue Konfigurator-LP** (W3–4, live ≤ 16.09.) | `/expertise/produktkonfigurator-roomle/` — 279 Impr. · Pos 8,4 (Hauptranking) | **301 in die LP** — Entscheidung Anatolij 17.08. (D5) |
| (Konfigurator, Zubringer) | — | `/newsroom/insights/onlineshop-produktkonfigurator/` — u. a. 55 Impr. · Pos 2,3; 65 · 7,4; trägt 1 von 2 Non-Brand-Klicks | Bleibt bestehen als Zubringer mit hartem Anker auf die LP; keine De-Optimierung (bestes Bestandsranking schützen) |
| produktkonfigurator (generisch, 502 Vol) | — | — | KEIN Ziel: Definitions-SERP (Software-Anbieter + Wikipedia + AI Overview, keine Agentur) |
| headless commerce agentur | `/expertise/commercetools/` (144 Impr. · Pos 22,4 — beste Headless-URL der Domain) | `/newsroom/news/frontastic/` — 1 Impr. · Pos 69 | Frontastic-URL: **301 auf Darkstar-Seite bei deren Livegang** (W5–7). Bis dahin trägt A1-Composable-Framing die Query |
| headless storefront · composable storefront | **Darkstar-Seite** (W5–7, 4 Blocker offen — D4) | — | Tracker ab Livegang; Bewertung Tag 90 |
| stammdatenmanagement · master data management | `/newsroom/insights/mdm/` | — | NUR interne Links (Datenwiderspruch; GSC gilt). Kein Refresh |
| agentur e-commerce strategie | — (Mapping aufgelöst, Council) | `/newsroom/insights/checklisteonlineshop/` — 89 Impr. · Pos 11,1 | Checkliste = Linkgeber-Asset; Refresh W8+ ohne Optimierung auf diese Query. Kommerzielles Ziel (Kandidat `/services/ecommerce-beratung/`) erst im Relaunch |

## 2. Relaunch-Backlog (Regeln jetzt, Umsetzung Relaunch)

| Query / Seite | Ziel-URL (Relaunch) | Regel |
| --- | --- | --- |
| conversion optimierung agentur (603 Impr. · Pos 69,5) | `/services/conversion-rate-optimization/` | Relaunch der bestehenden Seite (kein Neubau); im Fenster keine Maßnahme |
| b2b e-commerce agentur (6 Impr. · Pos 43,7) | `/expertise/b2b-ecommerce/` | Relaunch der bestehenden Seite |
| api-schnittstellen (2.400 Vol, KD 17) · schnittstellen programmierung (336 Impr · Pos 36,6) · api entwicklung (250 Vol, KD 0) · erp schnittstelle (150 Vol, KD 0) · datenintegration (800 Vol, KD 0) | **Neue Data-Hub-Seite** (`/expertise/schnittstellen/`) | `/expertise/api/` wird darauf konsolidiert. Insight-Artikel `apisunverzichtbar` bleibt Ranking-URL für `api schnittstellen agentur` und wird Zubringer. Konzept: [Relaunch-Briefing Data Hub](./relaunch-briefing-data-hub.md) |
| stammdaten- und produktdaten-Cluster (≈ 4.000 Impr auf `/newsroom/insights/mdm/`, u. a. `stammdatenmanagement` 819 · Pos 69,2, `e-commerce-datenmanagement` 251 · **Pos 3,6**, `produktdaten pflegen` 378 · **Pos 7,5**) | `/newsroom/insights/mdm/` bleibt | **Kein 301.** Die Seite hält zwei Spitzenpositionen und wird Zubringer mit hartem Anker auf die Data-Hub-Seite |
| `/expertise/b2c-ecommerce/` + `/expertise/direct-to-consumer-ecommerce/` | eine konsolidierte Seite | **Zusammenlegen per 301** (D6, 17.08.). Empfehlung: d2c bleibt (Insight „Direktvertrieb" trägt 2.713 Impr.), b2c wird 301 |
| `/expertise/omnichannel/` (61 Impr.) | `/expertise/omnichannel/` | Relaunch (D6); Insight `omnichannel-e-commerce/` (1.228 Impr. · Pos 18,9) wird Zubringer |
| `/expertise/website/` | → `/services/` | 301 (D6) |
| headless commerce (1.023 Vol) · agentic commerce (386) · composable commerce (267) | Headless-Hub + Satelliten + Agentic-Seite | Relaunch-Scope; Tracker läuft weiter |
| headless cms (1.812 Vol) | — | Nur beobachten (Vendor-SERP) |
| findologic | — | Gestrichen, nur Tracker |

## 3. Offen ([DU] / Folgetermine)

1. `/services-pre` (D7): Vorschauversion? Dann noindex + 301 auf `/services/`.
2. Darkstar-Blocker (D4) — bestimmen, wann die Zeilen „headless storefront/
   composable storefront" scharf werden.
3. Shopify-Case- und Partnerstatus-Namen (für Proof-Block W4).
4. Ziel der b2c/d2c-Konsolidierung final im Relaunch-Briefing bestätigen
   (Empfehlung oben: d2c bleibt).

Teil der Kosmonaut-Doku · Content-Plan Fassung 2 liegt im
kmt-website-fix-Repo (`docs/content-plan-60-tage`, `980b784`) ·
[Pilot](./pilot-kosmonaut-60-tage.md) · [Council + Addendum](./council-urteil-2026-08-17.md)
