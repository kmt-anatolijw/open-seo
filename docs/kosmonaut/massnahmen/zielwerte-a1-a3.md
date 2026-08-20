# Zielwerte Karten A1–A3 — Feld-für-Feld (Eingabe für die Klickvorlage)

Datenlieferung der SEO-Session an kmt-website-fix · 19.08.2026 · Weg (a)
Redakteursweg (D8). Soll-Werte aus der freigegebenen Kartendatei
[2026-08-16-cluster-a.md](./2026-08-16-cluster-a.md) (Freigabe D1, 17.08.).

**Ist-Werte regelkonform aus der Strapi-API** (Prod, `populate=deep`, ohne
`encodeSourceMaps`, 19.08.2026) — nicht aus dem HTML, gemäß der Direktive
vom 19.08. Alle Werte mit 0 unsichtbaren Zeichen; sie stimmen zeichengenau
mit der vorherigen HTML-Messung überein, die Zahlen ändern sich also nicht.

| Karte | Slug (Strapi-Format) | Page-ID |
| --- | --- | --- |
| A1 | `/expertise/commercetools` | **72** |
| A2 | `/expertise/oxid-shop` | **71** |
| A3 | `/newsroom/insights/apisunverzichtbar` | **66** |

Slugs führen einen Schrägstrich vorn und keinen hinten — der `trailingSlash`
der Website entsteht erst im Routing.

**Robots bleibt bei allen drei Karten unverändert:** live gemessen
`index, follow` auf allen drei URLs → `RobotsIndex = true`,
`RobotsFollow = true` nicht anfassen.

---

## ✅ Erledigt: Title-Suffix ist Content, nicht Code

**Aufgelöst 19.08.2026 durch die KMT-Session**, mit zwei unabhängigen Belegen:
`app/[[...rest]]/page.tsx:79` reicht `res.Title` unverändert an
`generateFullMetadata` weiter, in `utils/seo/metadata.ts` gibt es keine
Konkatenation, ein Next-`title.template` existiert nirgends im Repo;
Live-Gegenprobe zeichengleich mit dem Strapi-Feld.

**Folge:** Der Marken-Anhang „- Kosmonaut" steht redaktionell im Feld. Die
Soll-Titles müssen die Marke selbst mitbringen — `| KOSMONAUT` bleibt drin,
nichts wird gestrichen. Die Längen unten gelten wie gerechnet, und die
Marken-Dopplung bei A1 verschwindet mit dem neuen Wert von allein.

Damit wird die Längenfrage (D11) zur eigentlichen Entscheidung: die
freigegebenen Fassungen sind 77 bzw. 70 Zeichen lang und werden in der SERP
abgeschnitten.

## A1 — `https://kosmonaut.io/expertise/commercetools/`

| Feld | Wert |
| --- | --- |
| **Title (Soll, freigegeben)** | `commercetools Agentur – zertifizierter Partner für B2B, B2C & D2C \| KOSMONAUT` (77 Z.) |
| **Title (QS-Empfehlung, final)** | `commercetools Agentur & Solution Partner \| KOSMONAUT` (52 Z.) |
| Title (Alternative mit Zielgruppen) | `commercetools Agentur & Solution Partner für B2B & D2C \| KOSMONAUT` (66 Z.) |
| Title (Ist, Prod 19.08.) | `Kosmonaut - commercetools Partner und – maßgeschneiderter E-Commerce für B2B, B2C und D2C - Kosmonaut` (101 Z.) |
| **Meta-Description (Soll)** | `commercetools Agentur mit Implementierungs-Erfahrung: Composable Commerce für B2B, B2C und D2C — Beratung, Entwicklung, Betrieb. Jetzt Projekt anfragen.` (152 Z.) |
| Description (QS-Alternative mit Belegen) | `commercetools Solution Partner seit 2020, 11 Zertifikate im Team: Composable Commerce für B2B, B2C und D2C — Beratung, Entwicklung, Betrieb.` (140 Z.) |
| Description (Ist) | „Ein Shopsystem für den E-Commerce, das sich ganz nach dem Schwerpunkt…" (209 Z., abgeschnitten in der SERP) |
| **H1 (Soll)** | `commercetools Agentur für maßgeschneiderten E-Commerce (B2B, B2C, D2C)` |
| H1 (Ist) | `commercetools – maßgeschneiderter E-Commerce für B2B, B2C und D2C` |
| Robots | unverändert (`index, follow`) |

**QS-Flags A1**

1. **77 Zeichen sind zu lang** — die eigene A2-Karte fordert ≤ 65, und der
   Suffix ist Content, wird also mitgezählt. Empfehlung ist die 52-Zeichen-
   Fassung: `commercetools agentur` (445 + 475 Impr. auf zwei URLs) und
   `commercetools partner` (Pos 6,4) sind die einzigen Query-Gruppen mit
   belegter Nachfrage; B2B/B2C/D2C tauchen in den GSC-Queries dieser Seite
   nicht auf und kosten nur Zeichen. Zielgruppen-Fassung (66 Z.) als
   Alternative, falls die Positionierung im Title stehen soll.
2. **Statusbezeichnung.** „zertifizierter Partner" ist belegt, aber die
   offizielle Bezeichnung lautet **commercetools Solution Partner /
   Systems Integrator** (Tier laut Suchindex: Registered) (Beleg: `/newsroom/news/commercetools/` — Training und
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
| **Title (QS-Empfehlung, final)** | `OXID eShop Agentur & Diamant-Partner \| KOSMONAUT` (48 Z.) |
| Title (Soll, freigegeben — überholt) | `OXID Agentur \| Enterprise-Partner für B2B, B2C & D2C \| KOSMONAUT` (64 Z.) |
| Title (Ist, Prod 19.08.) | `Prämierte OXID Agentur \| Mehrfach ausgezeichnet für D2C, B2C und B2B Projekte \| Kosmonaut E-Commerce Agentur aus OWL - Kosmonaut` (128 Z.) |
| **Meta-Description (QS-Empfehlung, neu)** | `OXID Diamant-Partner seit 2025 · Partner seit 2008 · über 15 zertifizierte Entwickler · 80+ OXID-Projekte. Full-Service aus einer Hand.` (135 Z.) |
| Description (Ist, Prod) | „OXID Enterprise Partner. Über 15 Jahre OXID Erfahrung, mehr als 50 OXID Projekte, nur zertifizierte OXID Entwickler \| OXID eShop Full-Service aus einer Hand" (157 Z.) — **enthält die abgelöste Nomenklatur und eine widersprüchliche Projektzahl** |
| **H1 (Soll)** | `OXID Agentur – prämierte E-Commerce-Projekte auf OXID eShop` |
| H1 (Ist) | `OXID eShop skalierbare E-Commerce Plattform` |
| **Seitenleiste (dritte Stelle)** | führt ebenfalls „Enterprise-Partner seit: 2008" — muss im selben Zug auf „Diamant-Partner seit: 2008", sonst widerspricht die Seite sich selbst (Befund KMT) |
| Robots | unverändert (`index, follow`) |

**QS-Flags A2**

1. **Statusfrage geklärt (Anatolij, 19.08.2026): Kosmonaut ist OXID
   Diamant-Partner** — die höchste der aktuellen Stufen (Kristall → Rubin →
   Diamant). „Enterprise-Partner" ist die abgelöste Nomenklatur und gehört
   nicht mehr in den Title. Empfehlung oben führt stattdessen „Diamant-Partner".
2. Die Empfehlung deckt zusätzlich `oxid eshop agentur` ab (176 Impr./Pos 17,8,
   zweitgrößte Query der Seite, bisher in keinem Feld der Seite vorhanden) und
   `oxid partner` (24 Impr./Pos 20,8) — bei 48 Zeichen, also mit Reserve.
3. Die Ist-Description führt weiterhin „OXID Enterprise Partner". Sie sollte
   im selben Zug auf „Diamant-Partner" gezogen werden; Zertifizierungsbeleg
   `/newsroom/news/oxid-zertifizierung/` (Certified Development, 10 Entwickler,
   Oktober 2020 — Zahl laut Anatolij inzwischen höher, aktuelle Zahl nachtragen).
4. **Projektzahl entschieden (Anatolij, 19.08.2026): 80+.** Damit ist der
   Widerspruch gegenstandslos — Prod nannte 50+, die dev-Seitenleiste 40+,
   beides ist überholt. Die Zahl gehört in Description und Seitenleiste.
   Ebenfalls entschieden: **Shopware-Stufe Silver**, und der
   commercetools-Verzeichniseintrag ist unter der neuen URL
   `commercetools.com/service-partners/kosmonaut` wieder auflösbar
   (verifiziert 19.08., Partner tier „Registered").
5. Der Ist-Title trägt „Prämierte" — das entfällt im Soll. Bewusst so
   freigegeben (Intent vor Auszeichnung); die Auszeichnung bleibt in H1 und
   Description erhalten.

---

## A3 — `https://kosmonaut.io/newsroom/insights/apisunverzichtbar/`

| Feld | Wert |
| --- | --- |
| **Title (Soll, freigegeben)** | `APIs – Warum Schnittstellen unverzichtbar sind \| KOSMONAUT API-Agentur` (70 Z.) |
| **Title (final, nach Codex-Gate)** | `Warum APIs unverzichtbar sind \| KOSMONAUT` (41 Z.) |
| ~~Title (QS-Empfehlung, überholt)~~ | ~~`Warum APIs unverzichtbar sind \| KOSMONAUT API-Agentur` (53 Z.)~~ |
| Title (Ist, Prod 19.08.) | `APIs – Warum Schnittstellen unverzichtbar sind \| Kosmonaut E-Commerce Agentur aus OWL - Kosmonaut` (97 Z.) |
| **Meta-Description** | in der Karte nicht definiert. Vorschlag: `Wie APIs und Schnittstellen Systeme verbinden — und was eine API-Agentur bei ERP-, PIM- und Shop-Anbindung übernimmt. Über 70 API-Projekte.` (139 Z.) — Zahl belegt durch Anatolij, 19.08.2026 |

> **Korrektur 20.08. (PR #137, Codex-Gate):** Mein Suffix „| KOSMONAUT
> API-Agentur" ist gestrichen. Greptile und CodeRabbit haben unabhängig
> denselben Widerspruch gemeldet: **Council-Entscheidung C2** streicht genau
> diesen Anhang, weil er das Kontrollsignal `api-schnittstellen` gefährdet —
> der Title würde auf dieselbe Anfrage zielen, die der Artikel bereits ohne
> Zusatz trägt. Der Einwand ist berechtigt; ich hatte C2 bei der Empfehlung
> nicht berücksichtigt. Gültig ist `Warum APIs unverzichtbar sind | KOSMONAUT`
> (41 Zeichen).
>
> **Zur Eintragung:** Strapi nutzt Draft & Publish. Speichern allein genügt
> nicht — ein Save an einer publizierten Seite kann sie auf „Published
> (modified)" setzen, ohne die Live-Fassung zu ändern. Nach dem Speichern
> **veröffentlichen** und die Live-URL prüfen. Die Sichtbarkeit hängt zusätzlich
> am Revalidierungs-Webhook (Vorbedingung c in der Klickvorlage).
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

| # | Punkt | Wer | Stand |
| --- | --- | --- | --- |
| 1 | Title-Suffix im Code klären | KMT | **erledigt 19.08.** — kein Template, Suffix ist Content |
| 2 | OXID-Partnerstufe bestätigen | Anatolij | **erledigt 19.08.** — Diamant-Partner |
| 3 | Gekürzte Titles A1/A2/A3 freigeben | Anatolij | offen (D11) |
| 4 | A3-Description freigeben | Anatolij | offen (D12) |
| 5 | Aktuelle Zahl zertifizierter OXID-Entwickler | Anatolij | offen — für Title-Description und Schema |
| 6 | Shopware-Partnerstufe (Bronze/Silver/Gold/Platinum) | Anatolij | offen — fehlt im Schema |
| 7 | Sichtbaren Ankertext „commercetools Agentur" setzen | KMT | offen |
| 8 | Organization-Schema einbauen — Domain liefert null JSON-LD | KMT | offen, Vorlage liegt |

Teil der Kosmonaut-Doku · [Karten](./2026-08-16-cluster-a.md) ·
[Matrix](../keyword-url-matrix.md) · [Council + Addendum](../council-urteil-2026-08-17.md)
