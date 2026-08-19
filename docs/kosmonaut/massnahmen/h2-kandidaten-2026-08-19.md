# H2-Kandidaten — sieben Überschriften, die heute fehlen

19.08.2026 · SEO-Session · Datenlieferung, keine Vorgabe · Nachfolgepaket zu
den Karten A1–A3

> **Nachtrag 19.08. abends:** Nach der OpenSEO-MCP-Anbindung liegen jetzt auch
> Marktzahlen vor. Sie ändern die Reihenfolge — siehe
> [Marktdaten](../befunde-marktdaten-2026-08-19.md) und den Abschnitt
> „Reihenfolge nach Marktwert" weiter unten.

Kriterium: Die Suchanfrage bringt der Seite bereits Impressionen, das Wort
steht aber in **keiner** Überschrift. Aufwand je Position ein Absatz. Alle
Zahlen: GSC Service-Account, 90 Tage, Dimensionen query + page. Ist-Stand der
Überschriften: Live-HTML von Prod, 19.08.2026.

## Der Beleg, dass Überschriften auf diesen Seiten wirken

Zwei Kontrollfälle aus dem Bestand — beide unverändert lassen:

| Vorhandene Überschrift | Query | Position |
| --- | --- | --- |
| `<h2>` „Was sind APIs?" auf `/newsroom/insights/apisunverzichtbar/` | `was sind apis` | **11,2** |
| `<h3>` „OXID eShop in der Cloud" auf `/expertise/oxid-shop/` | `oxid cloud` | **7,8** — bester Wert der Seite |

Gegenprobe auf derselben Seite: Für `schnittstellen programmierung`
(336 Impressionen) existiert **keine** Überschrift — Position 36,6.
Gleiche Seite, gleiche Autorität, 25 Plätze Unterschied.

## Die sieben Kandidaten

| # | Neue Überschrift | Seite | Impr. | Pos. heute | Erwartung | Begründung |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | **Schnittstellenprogrammierung** | API-Artikel | 336 | 36,6 | 15–20 | Größte unbediente Query der Seite; zusätzlich `schnittstellenprogrammierung` (17 Impr., Pos 57,3) |
| 2 | **OXID eShop Agentur** | OXID | 176 | 17,8 | 8–12 | Zweite Schreibweise, steht in keinem Feld der Seite |
| 3 | **Headless Commerce Agentur** | commercetools | 138 | 22,6 | 10–14 | Heute nur die Ein-Wort-Überschrift `<h3>` „Headless" |
| 4 | **Welche Agentur ist Spezialist für Headless Commerce?** | commercetools | 35 | 9,9 | 3–6 | Wörtliche Nutzerfrage; zwei weitere Fragen derselben Art auf der Seite (Pos 1,0 und 10,6) |
| 5 | **Was ist commercetools?** | commercetools | 6 | 13,5 | 5–8 | Definitionsfrage ohne Antwortabschnitt; `commercetools beratung` (8 Impr., Pos 14,9) profitiert mit |
| 6 | **APIs im Shopsystem** | API-Artikel | 18 | 7,0–10,1 | 3–6 | `shopsystem apis` und `onlineshop apis` — beste Positionen der Seite, verbindet den Erklärartikel mit dem Kerngeschäft |
| 7 | **OXID Enterprise** | OXID | 15 | 13,7 | 6–9 | Nur umsetzen, wenn die Formulierung zur Diamant-Nomenklatur passt — siehe [Zielwerte](./zielwerte-a1-a3.md) |

Summe: rund 724 Impressionen, die heute auf Positionen liegen, von denen
praktisch niemand klickt.

Die Erwartungswerte sind aus den Positionssprüngen der beiden Kontrollfälle
oben abgeleitet, nicht aus Branchendurchschnitten.

## Reihenfolge nach Marktwert (Nachtrag 19.08.)

Die Impressionen oben sind eigene Messung. Was fehlte, war der Markt dahinter.
Aus DataForSEO, mit aufgelösten Singular-/Plural-Varianten:

| # | Kandidat | eigene Impr. | Volumen/Mon | Schwierigkeit | CPC |
| --- | --- | --- | --- | --- | --- |
| **3** | Headless Commerce Agentur | 138 | **210** | **1** | 25,49 € |
| **2** | OXID eShop Agentur | 176 | 40 (`oxid agentur`) | 9 | 21,01 € |
| **1** | Schnittstellenprogrammierung | 336 | **1** | 3 | 6,80 € |
| 7 | OXID Enterprise | 15 | 10 | — | — |
| 4, 5, 6 | Longtail-Fragen | 59 | kein messbares Volumen | — | — |

**Kandidat 1 fällt vom ersten auf den dritten Platz.** Er hatte die meisten
Impressionen und hat den kleinsten Markt: Aufgelöst wird
`schnittstellen programmierung` einmal im Monat gesucht. Das erklärt, warum die
336 Impressionen nie zu Klicks wurden — die Seite erscheint für ein Umfeld von
Varianten, von denen keine für sich genommen nachgefragt wird.

**Die Nachfrage liegt woanders:** `api schnittstelle` im **Singular** hat 2.397
Suchen im Monat bei Schwierigkeit 14 und transaktionaler Absicht. Der Plural
`api schnittstellen` hat eine.

**Empfehlung für Kandidat 1:** umsetzen, aber die Überschrift auf
**„API-Schnittstelle"** formulieren statt auf „Schnittstellenprogrammierung".
Gleicher Aufwand, ein Markt statt keiner.

Die Kandidaten 4, 5 und 6 bleiben sinnvoll — Longtail-Fragen haben selten
messbares Volumen, tragen aber Positionen zwischen 7,0 und 9,9 und sind genau
die Formulierungen, die auch an KI-Assistenten gehen.

## Offene Entscheidung: Ortsanfragen OXID

Kein Überschriften-Kandidat, sondern eine Richtungsfrage. Auf
`/expertise/oxid-shop/` sammeln sich Ortsanfragen:

| Query | Impr. | Pos. | Zusätzlich rankende URL |
| --- | --- | --- | --- |
| `oxid agentur münchen` | 96 | 15,9 | — |
| `oxid agentur allgäu` | 47 | 15,4 | `/newsroom/news/oxid-zertifizierung/` (50 Impr., Pos 12,0) |
| `oxid agentur kempten` | 33 | 14,8 | `/newsroom/news/oxid-zertifizierung/` (65 Impr., Pos 14,4) |
| `oxid agentur köln` | 14 | 18,1 | — |
| `oxid agentur stuttgart` + `oxid stuttgart` | 13 | 23,3 / 28,0 | — |

Rund 200 Impressionen aus Städten, in denen Kosmonaut keinen Standort hat.
Zwei saubere Wege: eine bundesweite Formulierung auf der Seite („deutschlandweit
für OXID-Projekte"), die den Ortsbezug ehrlich auflöst — oder bewusst liegen
lassen. **Nicht** empfohlen: Städtenamen ohne Standort in Überschriften, das
ist ein Muster, das Google als Doorway-Signal wertet.

## Messung

Je Position eigenes 28-Tage-Fenster ab Livegang. Kontrollsignale: Brand-Query
`kosmonaut` und die beiden Kontrollfälle oben dürfen nicht verlieren.
Baseline liegt unter [../data/](../data/) als GSC-Auszug vom 19.08.2026.

Teil der Kosmonaut-Doku · [Marktdaten](../befunde-marktdaten-2026-08-19.md) ·
[Zielwerte A1–A3](./zielwerte-a1-a3.md) ·
[Matrix](../keyword-url-matrix.md) · [Bericht](../reports/keyword-hebel.html)
