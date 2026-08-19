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

## Die Nutzlast wird erzeugt, aber von niemandem gelesen

Nachgefasst durch die KMT-Session: Die Kodierung braucht einen
Visual-Editing-Client, der sie wieder dekodiert. Den gibt es im Projekt nicht
— kein stega-/Visual-Editing-Konsument in `app/`, `components/`, `utils/`, und
im ausgelieferten dev-HTML kein Toolbar- oder Visual-Editing-Marker (deckt
sich mit meiner eigenen Messung).

Damit ist es kein genutztes Feature, sondern reiner Ballast, der zugleich jede
Längenmessung auf Preview-Deployments verfälscht. Anatolijs Grundsatz („solche
Steuerzeichen dürfen nicht vorkommen") wird deshalb nicht über ein Gate
erfüllt, sondern durch Entfernen der Quelle.

**Umgesetzt in PR #136:** `encodeSourceMaps` fliegt aus
`utils/queries/getPage/query.ts`; der Test kehrt die Richtung um und sichert
ab, dass der Parameter auch im Preview-Modus nicht mehr gesendet wird. Das ist
die Regressionsprüfung an der richtigen Stelle — ein Gate über gerenderte
Seiten entfällt.

**QS-Vorbehalt geprüft und ausgeräumt (19.08.2026):** Der Einwand lautete,
der Dekoder sei nicht der Projektcode, sondern die Vercel-Toolbar — die bei
angemeldeten Nutzern per Edge injiziert wird und deshalb in keiner
HTML-Messung auftaucht. Nachgeprüft: `@vercel/toolbar` ist **nicht
installiert**, im Repo liegen von Vercel nur `@vercel/analytics` und
`@vercel/speed-insights`; keine Content-Link-Konfiguration in
`next.config.mjs`, `vercel.json` oder im App-Code. Content Link braucht beide
Hälften — kodierte Inhalte **und** die eingerichtete Toolbar. Hier existiert
nur die kodierende. Auch angemeldete Nutzer bekommen kein Click-to-Edit.

Damit ist PR #136 eine Bereinigung, keine Funktionsentfernung. Sollte
Visual Editing später gewollt sein, kommt das Flag als bewusster Teil dieser
Einrichtung zurück statt als stiller Default mitzulaufen. Die Grundsatzfrage
— ist Visual Editing perspektivisch gewollt? — legt die KMT-Session Anatolij
vor dem Merge vor.

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
