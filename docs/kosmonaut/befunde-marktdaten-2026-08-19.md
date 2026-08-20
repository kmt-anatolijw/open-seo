# Marktdaten: was die Keywords wirklich wert sind

19.08.2026 · SEO-Session · Quelle: OpenSEO-MCP (self-hosted,
`openseo.tiaex.ai`) über DataForSEO, Projekt „KOSMONAUT Germany",
Markt Deutschland/Deutsch. Erste Abfrage nach der MCP-Anbindung.

Bisher lagen für die offenen Arbeitspakete nur **eigene** Nachfragedaten vor:
Impressionen aus der Search Console. Die sagen, wie oft Kosmonaut gesehen wird
— nicht, wie groß der Markt dahinter ist. Diese Zahlen schließen die Lücke.

## Vorweg: eine Messfalle, die zwei Zahlen erklärt

Google Ads meldet Singular und Plural als **eine** gruppierte Zahl und gibt sie
für beide Schreibweisen aus. Wer das nicht auflöst, hält jede Variante für
gleich stark. Der Clickstream-Abgleich trennt sie:

| Schreibweise | gruppiert | aufgelöst |
| --- | --- | --- |
| `api schnittstelle` (Singular) | — | **2.397** |
| `api schnittstellen` (Plural) | 2.400 | **1** |
| `pim system` | 1.300 | 1.189 |
| `erp schnittstelle` | 110 | 22 |
| `api entwicklung` | 110 | 55 |
| `datenintegration` | 320 | 320 |

**Verwertbar daraus:** Wo eine **neue Überschrift eine Suchanfrage tragen
soll**, heißt das Zielwort **`api schnittstelle`, im Singular.** Die Nachfrage
liegt praktisch vollständig dort.

*Reichweite der Regel, präzisiert am 20.08. (KMT):* Sie gilt für neue,
suchgetriebene Überschriften — **nicht rückwirkend für bereits beschlossene
Textbausteine.** Der in Abschnitt 4.3 des Plans festgelegte H2-Block
„KOSMONAUT als API- & Schnittstellen-Agentur" bleibt unberührt: Er benennt die
Leistung, er zielt nicht auf eine Anfrage. Der Einwand ist berechtigt — meine
ursprüngliche Formulierung war zu breit und hätte eine getroffene Entscheidung
nachträglich gekippt.

*Zur Zahl 2.400:* Sie ist ein **gruppierter Google-Ads-Wert** über die gesamte
Schreibweisenfamilie. Sie steht nicht im Widerspruch zur aufgelösten 1, sondern
misst etwas anderes — wo beide Zahlen in einem Dokument vorkommen, gehört diese
Kennzeichnung dazu.

## Die H2-Kandidaten mit Marktwert

Ergänzt die [H2-Liste](./massnahmen/h2-kandidaten-2026-08-19.md). Impressionen
= eigene GSC-Messung, 90 Tage. Volumen und Schwierigkeit = DataForSEO.

| # | Kandidat | eigene Impr. | Volumen/Mon | Schwierigkeit | CPC |
| --- | --- | --- | --- | --- | --- |
| 2 | OXID eShop Agentur | 176 | 10 (`oxid agentur`: 40) | 9 | 21,01 € |
| 3 | Headless Commerce Agentur | 138 | 210 (`headless commerce`) | **1** | 25,49 € |
| 6 | APIs im Shopsystem | 18 | — (Longtail) | — | — |
| 7 | OXID Enterprise | 15 | 10 | — | — |
| 1 | Schnittstellenprogrammierung | 336 | **1** (aufgelöst) | 3 | 6,80 € |
| 4 | Frage: Spezialist Headless Commerce | 35 | kein Volumen | — | — |
| 5 | Was ist commercetools? | 6 | kein Volumen | — | — |

**Kandidat 1 muss neu eingeordnet werden.** Er stand als größter Posten der
Liste — 336 Impressionen. Der Markt dahinter beträgt aufgelöst eine Suche pro
Monat.

Die beiden Zahlen widersprechen sich nur scheinbar: Die GSC-Impressionen sind
**gemessen** und betreffen die eigene Domain, das Suchvolumen ist
**modelliert**. Bei einem Konflikt gilt die Messung. Aber die Marktzahl erklärt,
warum die Impressionen nie zu Klicks werden: Google zeigt die Seite für ein
Umfeld von Varianten, von denen keine einzelne nennenswert gesucht wird.

**Konsequenz:** Kandidat 1 bleibt sinnvoll, aber nicht als größter Hebel — die
Überschrift sollte `API-Schnittstelle` tragen statt
`Schnittstellenprogrammierung`. Neue Reihenfolge nach Marktwert: **3, 2, 1**,
danach der Rest.

## Das Data-Hub-Umfeld: klein im Volumen, teuer im Klick

Korrigiert das [Data-Hub-Briefing](./relaunch-briefing-data-hub.md).

| Keyword | Volumen/Mon | Schwierigkeit | CPC | Absicht |
| --- | --- | --- | --- | --- |
| `api schnittstelle` | **2.397** | 14 | 2,72 € | transaktional |
| `pim system` | **1.189** | **1** | 16,23 € | informativ |
| `stammdatenpflege` | 390 | **0** | 1,83 € | informativ |
| `datenintegration` | 320 | **0** | — | informativ |
| `pim software` | 320 | **0** | 17,73 € | kommerziell |
| `stammdatenmanagement` | 320 | **0** | 9,91 € | informativ |
| `stammdaten management` | 320 | 1 | 9,91 € | kommerziell |
| `headless commerce` | 210 | 1 | 25,49 € | informativ |
| `replatforming` | 140 | **0** | 6,95 € | informativ |
| `shopify plus agentur` | 110 | **0** | 19,28 € | kommerziell |
| `shopify schnittstelle` | 90 | **0** | 10,74 € | navigational |
| `b2b shopsystem` | 70 | **0** | **34,79 €** | kommerziell |
| `erp anbindung` | 70 | **0** | **25,02 €** | navigational |
| `pim e-commerce` | 70 | **0** | 5,04 € | kommerziell |
| `api entwicklung` | 55 | **0** | 6,84 € | informativ |
| `shopware schnittstelle` | 50 | **0** | 4,73 € | kommerziell |
| `roqqio` | 50 | **0** | 0,18 € | informativ |
| `b2b shop software` | 40 | **0** | **41,60 €** | kommerziell |
| `erp schnittstelle` | 22 | **0** | — | informativ |
| `oxid schnittstelle` | 10 | — | 19,14 € | transaktional |

**Was daran zählt, ist nicht das Volumen, sondern die Spalte daneben.** Fast
das gesamte Feld hat Schwierigkeit **0 bis 1** — es gibt praktisch keine
etablierte Konkurrenz. Gleichzeitig zahlen Werbetreibende 25 bis 42 Euro pro
Klick. Ein Markt, in dem Anzeigen 40 Euro kosten und organisch niemand ernsthaft
steht, ist die Definition einer offenen Flanke.

Drei Korrekturen an früheren Angaben dieser Doku, alle nach unten:

| Angabe | bisher notiert | belegt |
| --- | --- | --- |
| `datenintegration` | 800/Monat | **320** |
| `api entwicklung` | 250/Monat | **55** |
| `erp schnittstelle` | 150/Monat | **22** |
| `shopware schnittstelle` | 100/Monat | **50** |

Die frühere Reihe stammte aus Ahrefs-Stichproben. Die Aussage des Briefings
bleibt bestehen — das Feld ist nachfrageseitig belegt und praktisch unbesetzt —
aber sie steht jetzt auf kleineren, dafür geprüften Zahlen.

**`data hub` bestätigt sich als Nicht-Ziel:** 1.000 Suchen im Monat, Absicht
aber **navigational** — gesucht werden fremde Produkte dieses Namens. Der
Begriff bleibt Beleg auf der Seite, nicht ihr Ziel-Keyword.

## Wo Kosmonaut organisch wirklich steht

| Domain | organischer Traffic/Mon | rankende Keywords |
| --- | --- | --- |
| dixeno.de | **2.281** | 604 |
| basecom.de | 960 | 284 |
| **kosmonaut.io** | **68** | **61** |
| blackbit.de | 32 | 7 |

Alle vier Werte aus derselben Quelle am selben Tag, damit vergleichbar.

**Nicht mit der [Wettbewerbstabelle](./wettbewerb-kosmonaut.md) vom 16.08.
mischen** — die stammt aus Ahrefs und liegt systematisch niedriger (dixeno dort
394 statt 2.281, kosmonaut.io 34 statt 68). Verschiedene Keyword-Datenbanken,
verschiedene Traffic-Modelle. Innerhalb einer Quelle sind die Verhältnisse
belastbar, über Quellen hinweg nicht.

Zwei Dinge sagen beide Quellen übereinstimmend:

1. **dixeno ist deutlich stärker** — und steht auf der OMR-OXID-Produktseite
   direkt neben Kosmonaut. Wer dort vergleicht, sieht zwei Namen; wer danach
   googelt, findet einen davon 33-mal häufiger.
2. **blackbit ist organisch schwächer als Kosmonaut** (32 Traffic, 7 Keywords) —
   hat aber **33 OMR-Bewertungen** gegen Kosmonauts 7. Das ist der Beleg, dass
   die OMR-Sichtbarkeit unabhängig von der organischen Stärke aufgebaut werden
   kann. Sie ist ein eigener Kanal, kein Nebenprodukt.

Punkt 2 stützt die [OMR-Strategie](./omr-strategie-2026-08-19.md) stärker als
alles bisher Gesammelte: Eine Agentur mit halb so viel organischem Traffic hat
dort das Fünffache an Stimmen.

## Was dixeno stark macht — und warum es kein Vorbild ist

Nachgezogen am 20.08.: die 602 rankenden Keywords von dixeno.de gegen die 61
von kosmonaut.io, jeweils die 60 traffic-stärksten.

**Dixenos Traffic ist ein Glossar-Effekt.** 40 der 60 stärksten Keywords stehen
auf `/ecommerce-lexikon/` — und tragen **1.635 von 1.913 Traffic-Einheiten,
also 85 Prozent**. Wofür sie ranken:

| Keyword | Vol/Mon | Pos | CPC |
| --- | --- | --- | --- |
| `adsense` | 9.900 | 7 | 0,51 € |
| `billing address` | 1.600 | 7 | **0,00 €** |
| `browser-caching` | 1.000 | 9 | 1,59 € |
| `billing address deutsch` | 720 | 6 | **0,00 €** |
| `what is a billing address` | 170 | 5 | **0,00 €** |

Wer „billing address deutsch" sucht, beauftragt keine E-Commerce-Agentur. Der
Klickwert liegt bei null, weil niemand darauf bietet. Dixeno hat 33-mal so viel
Traffic wie Kosmonaut — der größte Teil davon ist Lexikonverkehr ohne
Kaufabsicht.

**Kosmonaut steht kommerziell dichter, nur schlechter platziert:**

| | dixeno.de | kosmonaut.io |
| --- | --- | --- |
| Ø CPC der Top-60 | 4,71 € (Lexikon) / 2,44 € (Rest) | **7,86 €** |
| Keywords mit CPC über 10 € | 5 | **19** |
| organischer Traffic | 2.281 | 68 |

Das ist der eigentliche Befund: Nicht die Menge fehlt, sondern die Position.
Kosmonaut rankt für die richtigen, teuren Begriffe — durchgängig auf Seite drei
bis acht.

### Die sechs konkretesten Einzelchancen

Alle auf **bestehenden** Seiten, keine Neuanlage nötig:

| Keyword | Vol/Mon | Pos | CPC | Seite |
| --- | --- | --- | --- | --- |
| `shopware 6 agentur` | 170 | **47** | **73,39 €** | `/expertise/shopware-agentur/` |
| `plentymarkets` | 1.900 | **27** | 12,17 € | `/expertise/plentymarkets/` |
| `api-schnittstellen` | 2.400 | **32** | 2,72 € | `/newsroom/insights/apisunverzichtbar/` |
| `shopify plus agentur` | 110 | **34** | 19,28 € | `/expertise/shopify-agentur/` |
| `stammdatenmanagement` | 320 | **83** | 9,91 € | `/newsroom/insights/mdm/` |
| `e commerce systems` | 110 | 18 | 22,57 € | `/newsroom/insights/technologieeinsatz-im-ecommerce/` |

**`shopware 6 agentur` ist der teuerste Klick im gesamten Datensatz** — 73,39 €.
Die Seite existiert, der Partnerstatus existiert (Silver seit 2014), die
Nachfrage ist da. Position 47. Dieser Posten stand bisher in keinem Arbeitspaket.

**`api-schnittstellen` auf Position 32** ist die harte Zahl unter dem
[Data-Hub-Briefing](./relaunch-briefing-data-hub.md): Die Keyword-Familie trägt
2.400 Suchen im Monat, und der Erklärartikel steht bereits darin — nur zu weit
hinten, um gefunden zu werden.

### Das Strukturmuster bestätigt sich

Die Seiten mit den meisten rankenden Keywords sind durchweg **Artikel**, nicht
Angebotsseiten:

| Seite | rankende Keywords (Top-60) |
| --- | --- |
| `/newsroom/insights/direktvertrieb-im-e-commerce/` | 9 |
| `/newsroom/insights/mdm/` | 7 |
| Startseite | 7 |
| `/newsroom/insights/checklisteonlineshop/` | 5 |
| `/expertise/plentymarkets/` | 4 |

Vier der fünf stärksten Seiten verkaufen nichts. Genau die Diagnose, die die
[Matrix](./keyword-url-matrix.md) seit Beginn trägt — jetzt mit Marktzahlen
statt nur mit Impressionen belegt.

**Was daraus für den Relaunch folgt:** Kein Glossar bauen. Dixenos Weg erzeugt
Traffic, den man nicht verkaufen kann. Der kürzere Weg ist, die sechs Positionen
oben auf bestehenden Seiten zu heben — dort ist die Kaufabsicht bereits da.

## Methodisches für die nächsten Abfragen

- **Immer Clickstream mitziehen**, wenn eine Schreibweise zur Entscheidung wird.
  Ohne ihn zeigt jede Variante dieselbe gruppierte Zahl.
- **Eigene GSC-Messung schlägt modelliertes Volumen**, wenn beide vorliegen und
  sich widersprechen — das Volumen erklärt dann das Verhalten, ersetzt es nicht.
- **Quellen nicht mischen.** Jede Tabelle nennt ihre Herkunft und ihr Datum.

Teil der Kosmonaut-Doku · [H2-Kandidaten](./massnahmen/h2-kandidaten-2026-08-19.md) ·
[Data-Hub-Briefing](./relaunch-briefing-data-hub.md) ·
[OMR-Strategie](./omr-strategie-2026-08-19.md) ·
[Wettbewerb](./wettbewerb-kosmonaut.md)
