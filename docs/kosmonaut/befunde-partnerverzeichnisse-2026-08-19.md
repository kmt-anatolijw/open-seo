# Beleglage Partnerstatus — was extern überprüfbar ist und was nicht

19.08.2026 · SEO-Session · Anlass: PR #135 lässt `hasCredential` und `award`
bewusst weg, weil „die Partnerverzeichnisse heute keinen überprüfbaren
Eintrag liefern". Das stimmt in der Beobachtung, aber die Ursache liegt
woanders — und daraus folgt eine andere Empfehlung.

## Befund 1 — der commercetools-Eintrag ist nicht mehr erreichbar

| Abruf | Ergebnis |
| --- | --- |
| `commercetools.com/partners/kosmonaut` | **404**, `<title>Not Found</title>`, Canonical auf `/404` — auch im echten Browser (Playwright) |
| `commercetools.com/partners/shopmacher` | 404 |
| `commercetools.com/partners/kps-ag` | 404 |
| `commercetools.com/partners` (Übersicht) | 200 |

**Entscheidend ist die Kontrolle:** Auch die Seiten anderer, unstrittig
bestehender Partner geben 404. Es ist also kein Kosmonaut-spezifischer
Vorgang, sondern ein Website-Relaunch bei commercetools — das ausgelieferte
CSS heißt `commercetools-rebrand`, und das neue Partnerprogramm mit
Punktesystem stammt aus 2025. Die alte URL-Struktur `/partners/<name>`
existiert nicht mehr.

Google führt die alte Seite weiterhin im Index, mit Titel „Solution Partner
Kosmonaut | commercetools" und Inhaltsdetails: Solution Partner / Systems
Integrator, **Tier: Registered**, EMEA Germany, dazu die Leistungsliste.
Das aktuelle Verzeichnis lädt seine Partner dynamisch nach; ein statischer
Abruf zeigt überhaupt keine Namen, also auch keinen Gegenbeweis.

**Nicht belegbar ist derzeit beides** — weder dass der Eintrag besteht, noch
dass er fehlt.

## Befund 2 — das OXID-Verzeichnis nennt keine Partner mit Stufen

`oxid-esales.com/partner/partner-finden` ist erreichbar und nennt die
Programmstufen „Kristall, Rubin & Diamant", führt aber keine öffentliche
Liste einzelner Partner mit ihrer Stufe. Ein externer Beleg für „Diamant"
existiert also gar nicht — bei keinem OXID-Partner.

## Empfehlung: aufnehmen, aber als Selbstauskunft

Die Vorsicht in PR #135 ist grundsätzlich richtig — falsche Firmendaten
auszuliefern wäre schlimmer als unvollständige. Der Schluss „nicht extern
auflösbar, also weglassen" trägt hier aber nicht, aus zwei Gründen:

1. **Die übrigen Werte im selben Knoten sind ebenfalls Selbstauskunft.**
   Firmierung, Anschrift, USt-IdNr und Handelsregistereintrag stammen aus dem
   eigenen Impressum, nicht aus einem externen Register-Abruf. Der
   Partnerstatus steht auf derselben Stufe.
2. **Der Geschäftsführer hat ihn ausdrücklich erklärt** (19.08.2026: OXID
   Diamant-Partner; commercetools Solution Partner und Shopify Plus Partner
   bereits am 17.08.). Eine Erklärung des Unternehmens über sich selbst ist
   die belastbarere Quelle, wenn das Anbieterverzeichnis gerade umgebaut wird.

Konkret: `hasCredential` und `award` aufnehmen — aber **ohne `url`-Beleg**,
solange keiner auflöst. Ein `url`-Feld, das ins Leere zeigt, wäre schlechter
als keins. Sobald commercetools die neue Verzeichnis-URL hat, wird sie
nachgetragen.

Die Vorlage
[organization-kosmonaut.jsonld](./schema/organization-kosmonaut.jsonld) ist
entsprechend bereinigt: Der tote Verzeichnis-Link ist aus `url` und `sameAs`
entfernt, die commercetools-Credential trägt den Hinweis auf die Beleglage.

## Zwei Punkte für Anatolij

1. **Der commercetools-Partnereintrag ist nicht mehr erreichbar.** Google
   führt ihn noch, die Seite ist weg. Das ist ein verlorenes Entitätssignal
   und ein toter Verweis von einer sehr autoritativen Domain. Beim
   Partnermanagement ansprechen und die neue Verzeichnis-URL erfragen.
2. **Der indexierte Eintrag nennt „Registered" als Stufe.** Das ist die
   Einstiegsstufe des 2025er-Programms. Für den Title ist das unerheblich —
   „Solution Partner" ist die Rolle, nicht die Stufe — aber es ist der Stand,
   den ein Interessent findet, wenn er nachsieht.

Teil der Kosmonaut-Doku · [Schema](./schema/README.md) ·
[Zielwerte A1–A3](./massnahmen/zielwerte-a1-a3.md)
