# Zielwerte Karten A1–A3 — Feld-für-Feld (Eingabe für die Klickvorlage)

Datenlieferung der SEO-Session an kmt-website-fix · 19.08.2026 · Weg (a)
Redakteursweg (D8). Soll-Werte aus der freigegebenen Kartendatei
[2026-08-16-cluster-a.md](./2026-08-16-cluster-a.md) (Freigabe D1, 17.08.),
Ist-Werte live von Prod gezogen (fetch_page, 19.08.2026, HTTP 200).

**Robots bleibt bei allen drei Karten unverändert:** live gemessen
`index, follow` auf allen drei URLs → `RobotsIndex = true`,
`RobotsFollow = true` nicht anfassen.

---

## ⚠ Vorab-Befund: Title-Suffix wird vom Code angehängt

Alle drei Prod-Titles enden auf `- Kosmonaut`, zwei zusätzlich auf
`| Kosmonaut E-Commerce Agentur aus OWL`. A1 führt „Kosmonaut" dadurch
doppelt. Das heißt: der Wert im Strapi-Title-Feld ist **nicht** der Title in
der SERP — ein Template hängt an.

**Vor dem Eintragen zu klären (KMT, Code):** Wie lautet das
`metadata.title.template` bzw. der Suffix-Aufbau? Danach gilt für die
Vorlage: Wenn der Code die Marke anhängt, wird `| KOSMONAUT` aus den
Soll-Titles unten **gestrichen** (sonst doppelte Marke und >80 Zeichen). Die
Längenangaben unten sind ohne Suffix gerechnet.

---

## A1 — `https://kosmonaut.io/expertise/commercetools/`

| Feld | Wert |
| --- | --- |
| **Title (Soll, freigegeben)** | `commercetools Agentur – zertifizierter Partner für B2B, B2C & D2C \| KOSMONAUT` (77 Z.) |
| **Title (QS-Variante, empfohlen)** | `commercetools Agentur & Solution Partner für B2B & D2C \| KOSMONAUT` (66 Z.) |
| Title (Ist, Prod 19.08.) | `Kosmonaut - commercetools Partner und – maßgeschneiderter E-Commerce für B2B, B2C und D2C - Kosmonaut` (101 Z.) |
| **Meta-Description (Soll)** | `commercetools Agentur mit Implementierungs-Erfahrung: Composable Commerce für B2B, B2C und D2C — Beratung, Entwicklung, Betrieb. Jetzt Projekt anfragen.` (152 Z.) |
| Description (Ist) | „Ein Shopsystem für den E-Commerce, das sich ganz nach dem Schwerpunkt…" (209 Z., abgeschnitten in der SERP) |
| **H1 (Soll)** | `commercetools Agentur für maßgeschneiderten E-Commerce (B2B, B2C, D2C)` |
| H1 (Ist) | `commercetools – maßgeschneiderter E-Commerce für B2B, B2C und D2C` |
| Robots | unverändert (`index, follow`) |

**QS-Flags A1**

1. **77 Zeichen sind zu lang** — die eigene A2-Karte fordert ≤ 65. Empfehlung:
   QS-Variante (66 Z.) oder `commercetools Agentur – Solution Partner | KOSMONAUT` (52 Z.).
2. **Statusbezeichnung.** „zertifizierter Partner" ist belegt, aber die
   offizielle Bezeichnung lautet **commercetools Solution Partner /
   Systems Integrator** (Beleg: `/newsroom/news/commercetools/` — Training und
   Zertifizierung der Developer + Solution/Functional Architects abgeschlossen
   **Dezember 2020**). Empfehlung: offizielle Bezeichnung verwenden.
   **Aktualität ist zu bestätigen** (Beleg ist 5 Jahre alt) — auf die
   gebündelte Entscheidungsliste.
3. **Zubringer-Anker fehlt sichtbar.** `/newsroom/news/commercetools/`
   verlinkt bereits 3× auf die Zielseite, aber mit den Ankertexten
   „commercetools Solution Partner" / „commercetools Partner" — „commercetools
   Agentur" steht nur im `title`-Attribut. Matrix-Regel verlangt den harten
   Anker im **sichtbaren** Text: einen der drei Anker auf „commercetools
   Agentur" ändern. Relevanz: Die News-Seite rankt für die Fokus-Query besser
   als das Ziel (472 Impr./Pos 13,8 vs. 432/15,3).
4. Composable-Framing im Body ist die tragende Headless-Maßnahme des Fensters
   (Council-Addendum 3); der Link commercetools → Darkstar kommt erst zum
   Livegang der Darkstar-Seite (W5–7).

---

## A2 — `https://kosmonaut.io/expertise/oxid-shop/`

| Feld | Wert |
| --- | --- |
| **Title (Soll, freigegeben)** | `OXID Agentur \| Enterprise-Partner für B2B, B2C & D2C \| KOSMONAUT` (64 Z.) |
| Title (Ist, Prod 19.08.) | `Prämierte OXID Agentur \| Mehrfach ausgezeichnet für D2C, B2C und B2B Projekte \| Kosmonaut E-Commerce Agentur aus OWL - Kosmonaut` (128 Z.) |
| **Meta-Description** | **unverändert lassen** — Ist ist gut (157 Z.): „OXID Enterprise Partner. Über 15 Jahre OXID Erfahrung, mehr als 50 OXID Projekte, nur zertifizierte OXID Entwickler \| OXID eShop Full-Service aus einer Hand" |
| **H1 (Soll)** | `OXID Agentur – prämierte E-Commerce-Projekte auf OXID eShop` |
| H1 (Ist) | `OXID eShop skalierbare E-Commerce Plattform` |
| Robots | unverändert (`index, follow`) |

**QS-Flags A2**

1. „Enterprise-Partner" ist kein neuer Claim — die Ist-Description führt ihn
   bereits. Zertifizierungsbeleg: `/newsroom/news/oxid-zertifizierung/`
   („OXID eShop Enterprise Edition Certified Development", 10 Entwickler,
   Oktober 2020). Aktualität ebenfalls bestätigen lassen.
2. Der Ist-Title trägt „Prämierte" — das entfällt im Soll. Bewusst so
   freigegeben (Intent vor Auszeichnung); die Auszeichnung bleibt in H1 und
   Description erhalten.

---

## A3 — `https://kosmonaut.io/newsroom/insights/apisunverzichtbar/`

| Feld | Wert |
| --- | --- |
| **Title (Soll, freigegeben)** | `APIs – Warum Schnittstellen unverzichtbar sind \| KOSMONAUT API-Agentur` (70 Z.) |
| **Title (QS-Variante)** | `Warum APIs unverzichtbar sind \| KOSMONAUT API-Agentur` (53 Z.) |
| Title (Ist, Prod 19.08.) | `APIs – Warum Schnittstellen unverzichtbar sind \| Kosmonaut E-Commerce Agentur aus OWL - Kosmonaut` (97 Z.) |
| **Meta-Description** | in der Karte nicht definiert. Vorschlag zur Freigabe: `Wie APIs und Schnittstellen Systeme verbinden — und was eine API-Agentur bei ERP-, PIM- und Shop-Anbindung übernimmt. Praxis aus über 50 Projekten.` (149 Z.) |
| Description (Ist) | „API oder Schnittstellen verbinden externe Dienste oder Software um Datenpunkte zu verknüpfen und Prozesse zu automatisieren." (125 Z.) |
| **H1** | unverändert (`APIs – Warum Schnittstellen unverzichtbar sind`) |
| **Neuer H2-Block im Body** | `KOSMONAUT als API- & Schnittstellen-Agentur` — Leistungsversprechen (Systemintegration, Middleware, ERP/PIM/Shop-Anbindung), 2–3 Projekt-Referenzen, CTA auf die Kontaktseite |
| Robots | unverändert (`index, follow`) |

**QS-Flags A3**

1. Der CTA verlinkt auf die **Kontaktseite, nicht auf `/expertise/api/`**
   (Matrix: die Insight-URL ist die dauerhafte Ranking-URL; `/expertise/api/`
   ist Relaunch-Konsolidierungsziel und bekommt im Fenster keine Optimierung
   auf diese Query).
2. Der H2-Block ist Body-Inhalt, kein Meta-Feld — je nach CMS-Aufbau eine
   Dynamic-Zone-Komponente, nicht Teil der Klickvorlage im engeren Sinn.
3. Die Description ist **nicht freigegeben** (Karte definiert keine) — der
   Vorschlag braucht ein OK, bevor er in die Vorlage geht.

---

## Zusammenfassung der offenen Punkte

| # | Punkt | Wer |
| --- | --- | --- |
| 1 | Title-Suffix im Code klären, danach `\| KOSMONAUT` in den Soll-Titles streichen oder behalten | KMT |
| 2 | Gekürzte Title-Varianten A1/A3 freigeben (freigegebene Fassung ist 77 bzw. 70 Z.) | Anatolij |
| 3 | A3-Description freigeben (neu vorgeschlagen) | Anatolij |
| 4 | Aktualität commercetools-Solution-Partner- und OXID-Enterprise-Status bestätigen | Anatolij |
| 5 | Sichtbaren Ankertext „commercetools Agentur" in `/newsroom/news/commercetools/` setzen | KMT |

Teil der Kosmonaut-Doku · [Karten](./2026-08-16-cluster-a.md) ·
[Matrix](../keyword-url-matrix.md) · [Council + Addendum](../council-urteil-2026-08-17.md)
