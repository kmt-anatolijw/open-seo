# dev-Steuerzeichen: aufgeklärt — Editier-Overlay, kein Defekt

19.08.2026 · SEO-Session (Messung) + KMT-Session (Ursachenklärung) ·
**Ergebnis: kein Handlungsbedarf am Code, aber eine bindende Messregel.**

## Was gemessen wurde

| Abrufweg | Seite | Unsichtbare Zeichen |
| --- | --- | --- |
| einfacher Fetcher | dev `/expertise/commercetools/` | 36.044 |
| Playwright, echter Chrome | dev `/expertise/commercetools/` | 36.044 — zeichengenau identisch |
| einfacher Fetcher | dev `/expertise/oxid-shop/` | 48.416 |
| Playwright | Prod `/expertise/commercetools/` | 0 |

62 Sequenzen mit 20 und mehr Zeichen, die längste 632, jede am Ende eines
Textblocks unmittelbar vor dem schließenden Tag. Auch im Next-Flight-Payload
enthalten, also serverseitig.

## Ursache (KMT, code-belegt und reproduziert)

`utils/queries/getPage/query.ts:21` sendet an Strapi:

```ts
encodeSourceMaps: process.env.VERCEL_ENV === "preview"
```

Das Strapi-`sourcemaps`-Plugin kodiert daraufhin die Editier-Herkunft jedes
Textwerts steganografisch in den Text — das Verfahren hinter Vercels Visual
Editing, das ZWSP, ZWNJ, ZWJ und BOM als Trägerzeichen nutzt. Direkt
reproduziert: dieselbe API-Abfrage mit `&encodeSourceMaps=true` liefert die
Hero-Headline mit 657 statt 65 Zeichen, davon 592 unsichtbar — exakt die
Zahl, die im gerenderten `<h1>` gemessen wurde.

Ausgeschlossen wurde davor:

- **Redaktionsinhalt**: dev-Strapi ohne das Flag abgefragt (`populate=deep,10`,
  9 Komponenten) → 0 unsichtbare Zeichen.
- **Code**: `grep` über app/components/utils nach den vier Codepoints →
  0 Treffer; lokaler Render der Hero-Komponente → sauber.
- **Umgebungsspezifik**: frisches Preview-Deployment (PR #135) → dieselben
  36.044, also deterministisch.
- **Mein Abrufwerkzeug**: zwei verschiedene Clients, identische Zahl.

`dev.kosmonaut.io` läuft als `VERCEL_ENV=preview`, Prod als `production` —
dort ist das Flag konstruktionsbedingt aus. Deshalb 0 Zeichen auf Prod.

## Kein Gate

Eine Prüfung, die unsichtbare Steuerzeichen abweist, würde auf dev jeden Build
zu Recht rot färben und ein gewolltes Editier-Feature bekämpfen. Auf Prod kann
das Overlay bauartbedingt nicht erscheinen. Der Grundsatz „solche Zeichen
dürfen nicht vorkommen" (Anatolij, 19.08.) bleibt richtig für alles, was auf
Prod landet — hier greift er nicht, weil nichts davon je nach Prod gelangt.

## Bindende Messregel

**Jede Zeichen-, Längen- oder Tokenisierungsmessung auf dev muss die
unsichtbaren Zeichen vorher entfernen.** Sonst wird das Editier-Overlay
gemessen statt des Inhalts.

Betroffen sind unmittelbar:

- **Title- und Description-Längen** — die ≤ 65-Zeichen-Regel für Titles wäre
  auf dev-Werten sinnlos.
- H2- und Überschriften-Analysen.
- Dateigrößen und Wortzahlen.

Filter vor jeder Auswertung: alle Zeichen aus U+200B, U+200C, U+200D und
U+FEFF strippen.

```python
INVIS = {"​", "‌", "‍", "﻿"}
clean = "".join(ch for ch in raw if ch not in INVIS)
```

Nicht betroffen: Struktur- und Markup-Prüfungen (Schema, semantische
Landmarks, Robots-Header) — dort stören die Zeichen nicht.

## Was der Vorgang gezeigt hat

Die Messung war korrekt und über drei Wege belastbar; die erste
Ursachenvermutung („Wasserzeichen oder Redaktionsartefakt, Gate nötig") war
falsch. Geklärt hat es der direkte API-Call gegen Strapi — angeordnet von
Anatolij, ausgeführt von der KMT-Session. Für künftige Fälle: Ein Effekt, der
nur zwischen zwei Umgebungen auftritt, wird an der Datenquelle isoliert, nicht
am ausgelieferten HTML.

## Nebenbefund: zwei getrennte CMS-Instanzen

`dev.kosmonaut.io` zieht aus `dev-strapi.kosmonaut.io`, Prod aus
`strapi.kosmonaut.io`.

Konsequenz für die Karten A1–A3: Titles, Descriptions und H1 sind
**CMS-Inhalte und nicht vom main-Port abhängig** — im Prod-Strapi gepflegt
wirken sie sofort live. Ob ein automatischer Abgleich zwischen den Instanzen
läuft, verifiziert die KMT-Session vor der Klickvorlage; der bisherige Stand
ist, dass es keinen gibt und die Inhalte real auseinanderdriften.

Teil der Kosmonaut-Doku · [Schema-Befund](./schema/README.md) ·
[Zielwerte A1–A3](./massnahmen/zielwerte-a1-a3.md)
