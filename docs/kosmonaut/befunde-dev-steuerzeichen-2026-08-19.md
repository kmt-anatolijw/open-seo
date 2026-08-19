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

## Bewertung

1. **Die dev-Seite ist mehr als doppelt so groß wie dieselbe Seite auf Prod**
   — 212 KB statt 97 KB, und der Unterschied besteht praktisch vollständig
   aus unsichtbaren Zeichen. Das sind rund 17 % reiner Ballast.
2. **Das Muster deutet auf ein Wasserzeichen oder ein Werkzeug-Artefakt.**
   Lange gemischte Sequenzen aus ZWNJ/ZWJ/ZWSP/BOM hinter Textblöcken sind
   die typische Form unsichtbarer Textmarkierung. Sie entstehen nicht
   zufällig und nicht beim normalen Redigieren.
3. **Prod ist sauber.** Der Effekt kommt also aus dem dev-Zweig oder aus dem
   dev-Strapi — und würde mit dem main-Port auf die Live-Seite gelangen.
4. **Risiko für die Textverarbeitung.** Solange die Zeichen hinter dem Text
   stehen, ist die Tokenisierung der Überschriften vermutlich unbeschädigt.
   Stünden sie zwischen Buchstaben, wären die Überschriften für Suchmaschinen
   keine erkennbaren Wörter mehr. Das ist zu prüfen, bevor portiert wird.

## Empfehlung

Vor dem main-Port klären: Woher stammen die Sequenzen — Redaktionsinhalt im
dev-Strapi, ein Build-Schritt oder ein Editor-Werkzeug? Danach entfernen und
eine Prüfung in die Gates aufnehmen, die unsichtbare Steuerzeichen in
Überschriften und Meta-Feldern abweist.

Das gehört zu der Bedingung „Komponenten und Patterns sauber und fehlerfrei"
(Anatolij, 19.08.2026), die vor dem Port erfüllt sein soll.

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
