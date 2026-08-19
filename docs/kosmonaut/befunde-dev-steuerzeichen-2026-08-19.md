# Befund dev: 36.044 unsichtbare Steuerzeichen pro Seite

19.08.2026 · SEO-Session · gemessen nach der neuen Messregel gegen
`dev.kosmonaut.io` · Gegenmessung Prod

## Messung

| Umgebung | URL | Dateigröße | Unsichtbare Steuerzeichen |
| --- | --- | --- | --- |
| **dev** | `dev.kosmonaut.io/expertise/commercetools/` | 212.046 Zeichen | **36.044** |
| **Prod** | `kosmonaut.io/expertise/commercetools/` | 96.589 Zeichen | **0** |

Aufschlüsselung dev: 11.542 Zero-Width-Non-Joiner (U+200C), 9.776
Zero-Width-Joiner (U+200D), 7.686 Zero-Width-No-Break-Space / BOM (U+FEFF),
7.040 Zero-Width-Space (U+200B).

Verteilung: In **jeder** Überschrift. Das `<h1>` trägt 592 solcher Zeichen,
die geprüften `<h2>` jeweils 564 bis 596. Sie stehen als zusammenhängende
Sequenz **hinter** dem sichtbaren Text, nicht zwischen den Buchstaben.

## Verifikation (nachgeschärft 19.08.2026)

Auf Nachfrage von Anatolij über drei unabhängige Wege gegengeprüft:

| Abrufweg | Seite | Unsichtbare Zeichen |
| --- | --- | --- |
| einfacher Fetcher | dev `/expertise/commercetools/` | 36.044 |
| **Playwright, echter Chrome** | dev `/expertise/commercetools/` | **36.044 — identisch** |
| einfacher Fetcher | dev `/expertise/oxid-shop/` | 48.416 |
| Playwright, echter Chrome | Prod `/expertise/commercetools/` | 0 |

Zwei völlig verschiedene Clients liefern zeichengenau dieselbe Zahl. Ein
Artefakt des Abrufwerkzeugs ist damit ausgeschlossen. Der Effekt ist real und
auf dev reproduzierbar.

## Wo genau die Sequenzen sitzen

**62 Sequenzen** mit jeweils 20 und mehr Zeichen, die längste 632 Zeichen.
Jede sitzt **am Ende eines Textblocks, unmittelbar vor dem schließenden Tag**:

```
…für B2B, B2C und D2C[592 unsichtbare Zeichen]</h1>
…mit dem Fokus auf eine einzigartige Customer Experience.[576]</p>
```

Sie sind auch im Next-Flight-Payload enthalten (18.640 von 103.571 Zeichen)
— also in den serialisierten **Serverdaten**, nicht erst im Browser-DOM.
Damit ist eine clientseitige Injektion ausgeschlossen.

**Widerlegte Hypothese:** Kein Vercel-Toolbar-, Preview- oder
Draft-Mode-Marker im Dokument (`vercel`, `toolbar`, `speed-insights`,
`draft`, `live-preview` kommen auf dev wie auf Prod null Mal vor). Es ist
also kein Preview-only-Feature der Hosting-Umgebung.

## Offen: Strapi oder Render-Schritt?

Die Sequenzen hängen an jedem Rich-Text-Block. Eine Stichprobe auf die
Strapi-Feldnamen im Payload (`Headline`, `Title`, `Text`, `Subline`,
`MetaDescription`, `BreadcrumbText`) fand sie dort **nicht** — dieser Test ist
aber zu schwach, um Strapi freizusprechen: Er greift nur bei kurzen Werten
und einer bestimmten Serialisierungsform.

**Die Frage lässt sich nur an der Quelle entscheiden.** Prüfauftrag an die
KMT-Session (Anatolij, 19.08.2026): direkter API-Call gegen
`dev-strapi.kosmonaut.io` für den commercetools-Eintrag, Response auf
U+200B, U+200C, U+200D und U+FEFF prüfen; dieselbe Prüfung gegen
`strapi.kosmonaut.io`.

- **Treffer im Strapi-Response** → die Zeichen stecken im Redaktionsinhalt.
  Herkunft klären (aus welchem Werkzeug wurden die Texte eingefügt?), Felder
  bereinigen, Eingangsprüfung ergänzen.
- **Strapi-Response sauber** → ein Schritt zwischen Datenabruf und Auslieferung
  fügt sie ein. Dann die Markdown- oder Rich-Text-Verarbeitung prüfen.

## Grundsatz (Anatolij, 19.08.2026)

**Solche Steuerzeichen dürfen nicht vorkommen** — unabhängig davon, welche
Stufe sie einträgt. Nach der Ursachenklärung gehört eine Prüfung in die
Gates, die unsichtbare Steuerzeichen in Überschriften, Meta-Feldern und
Fließtext abweist.

## Warum das zählt

1. Die dev-Seite ist mehr als doppelt so groß wie dieselbe Seite auf Prod —
   212 KB statt 97 KB, praktisch vollständig unsichtbarer Ballast.
2. Solange die Zeichen **hinter** dem Text stehen, ist die Worterkennung
   vermutlich unbeschädigt. Stünden sie zwischen Buchstaben, wären die
   Überschriften für Suchmaschinen keine erkennbaren Wörter mehr. Das ist vor
   einem Port zu prüfen.
3. Jede Content-Messung auf dev — Textlänge, Lesbarkeit, Keyword-Dichte —
   ist verzerrt, solange das besteht.

## Nebenbefund: zwei getrennte CMS-Instanzen

`dev.kosmonaut.io` zieht aus `dev-strapi.kosmonaut.io`, Prod aus
`strapi.kosmonaut.io`. Inhaltlich sind beide beim geprüften Datensatz
identisch (Title, Description und H1 zeichengleich).

Konsequenz für die Karten A1–A3: Änderungen an Titles, Descriptions und H1
sind **CMS-Inhalte und nicht vom main-Port abhängig** — im Prod-Strapi
gepflegt wirken sie sofort live. Offen ist, in welche Richtung die beiden
Instanzen abgeglichen werden: Wird dev regelmäßig aus Prod geklont, gehen
dort gepflegte Texte verloren; läuft es umgekehrt, überschreibt ein Sync die
Prod-Texte. **Diese Frage gehört beantwortet, bevor die Klickvorlage
abgearbeitet wird** — sonst ist die Arbeit beim nächsten Abgleich weg.

Teil der Kosmonaut-Doku · [Schema-Befund](./schema/README.md) ·
[Zielwerte A1–A3](./massnahmen/zielwerte-a1-a3.md)
