# Strukturierte Daten kosmonaut.io — Befund und Vorlagen

Stand 19.08.2026 · SEO-Session · **korrigierte Fassung**

## Messregel (Anatolij, 19.08.2026)

Technische Messungen — Schema, Markup, Rendering, Agent-UX, Header — laufen
gegen **dev.kosmonaut.io**. Dort liegt der aktuelle Code. Sichtbarkeit und
Nachfrage — GSC, Ahrefs, Rank-Tracker, Index-Status — bleiben bei **Prod**,
weil dev zwangs-noindex ist und keine Messdaten hat. Findet sich etwas nur auf
Prod und nicht auf dev, ist es kein Code-Befund, sondern der fehlende
main-Port.

## Zwei getrennte Befunde

### Befund 1 — Prod liefert nichts, obwohl der Code fertig ist

| Umgebung | Geprüfte Seiten | JSON-LD |
| --- | --- | --- |
| **Prod** (`kosmonaut.io`) | Startseite, `/expertise/commercetools/`, `/expertise/oxid-shop/`, `/newsroom/insights/apisunverzichtbar/` | **0 Blöcke** |
| **dev** (`dev.kosmonaut.io`) | `/expertise/commercetools/` | **3 Blöcke**: Organization, WebSite, BreadcrumbList |

Prod-Messung zweifach verifiziert (Roh-HTML via `fetch_page.py`, gerendertes
DOM via `render_page.py` mit Playwright). dev-Gegenmessung am 19.08. durch
diese Session bestätigt.

**Ursache (KMT-Session, code-belegt):** `git show origin/dev:app/layout.tsx |
grep -c StructuredData` → 3, dasselbe gegen `origin/main` → 0. Die Einbindung
existiert auf `dev`, nicht auf `main`; Prod läuft auf `main` und liegt rund
107 Commits zurück.

**Konsequenz:** Kein Arbeitspaket für die Entwicklung, sondern Evidenz für D9
(terminierter main-Port). Organization, WebSite und BreadcrumbList fehlen auf
allen ~107 Prod-Seiten, obwohl der Code seit Langem steht. Derselbe Mechanismus
wie beim Breadcrumb-404 aus PR #101.

*Korrektur:* Die erste Fassung dieses Dokuments schloss aus der Prod-Messung
auf einen Einbaufehler. Das war falsch — die Messung stimmte, die
Schlussfolgerung nicht. Council-Addendum Punkt 5 war richtig, es beschrieb den
dev-Stand.

### Befund 2 — auch auf dev trägt das Organization-Schema keine Substanz

Gemessen am 19.08.2026 auf `dev.kosmonaut.io/expertise/commercetools/`:

```json
{ "@context": "https://schema.org", "@type": "Organization",
  "name": "Kosmonaut", "url": "https://dev.kosmonaut.io" }
```

Vier Felder. Keine Anschrift, keine Partnerstatus, keine Auszeichnungen, kein
`sameAs`. Damit erfüllt der Knoten seinen eigentlichen Zweck nicht: Er ist
kein Entitäts-Anker, weil nichts ihn mit dem commercetools-Partnerverzeichnis,
dem LinkedIn-Profil oder dem Handelsregistereintrag verbindet.

Konkret unsichtbar bleiben: **OXID Diamant-Partner** (höchste Programmstufe),
commercetools Solution Partner / Systems Integrator, Shopify Plus Partner,
OXID eShop Enterprise Edition Certified Development, vierfacher Gewinner und
Agentur des Jahres beim Shop Usability Award. Alles nur Fließtext.

BreadcrumbList dagegen ist vollständig und korrekt (dreistufig, mit `@id` je
Ebene). Da ist nichts zu tun.

## Vorlage — Organization mit Substanz

[organization-kosmonaut.jsonld](./organization-kosmonaut.jsonld) ersetzt den
Vier-Felder-Stub. Ergänzt gegenüber dem heutigen Stand: `address`,
`telephone`, `vatID`, `founder`, `award`, `hasCredential` (vier Partnerstatus
mit `recognizedBy`), `knowsAbout`, `sameAs`, `areaServed`.

Belegt durch: Impressum (Firmierung, Anschrift, USt-IdNr, Geschäftsführung),
Site-Banner (Awards), `/newsroom/news/commercetools/` und
`commercetools.com/partners/kosmonaut` (Solution Partner),
`/newsroom/news/oxid-zertifizierung/` (Certified Development),
Anatolij 19.08.2026 (OXID Diamant), D3-Freigabe 17.08.2026 (Shopify Plus).

**Bewusst leer gelassen:**

- Shopware-Partnerstufe — Shopware führt seit 2026 Bronze/Silver/Gold/Platinum;
  welche gilt, ist offen. Lieber leer als falsch.
- Zahl der zertifizierten OXID-Entwickler — die Meldung von 2020 nennt zehn,
  laut Anatolij sind es inzwischen mehr. Gehört in die `description` der
  Certified-Development-Credential; eine Zahl wirkt in Trefferliste und
  KI-Antworten stärker als ein Adjektiv.

## Einbaureihenfolge der Credentials — Schema darf der Seite nicht widersprechen

Festgelegt mit der KMT-Session am 19.08.2026. Leitsatz: **Die maschinenlesbare
Aussage muss die sichtbare Seite abbilden, nicht ihr vorauseilen.** Ein
`hasCredential` „Diamant-Partner" auf einer Seite, deren Seitenleiste
„Enterprise-Partner" sagt, ist ein Widerspruch auf derselben URL — und genau
das, was Googles Richtlinien für strukturierte Daten untersagen.

| Credential | Aufnahme | Bedingung |
| --- | --- | --- |
| commercetools Solution Partner (seit 2020, 11 Zertifikate) | **sofort** | `/newsroom/news/commercetools/` nennt „Solution Partner" zweimal, „Systems Integrator" einmal; die Seitenleiste nennt Jahr und Zertifikate |
| Shopify Plus Partner (seit 2020) | **sofort** | Die Shopify-Seite führt „Shopify Plus" zehnmal |
| OXID Diamant-Partner (seit 2008) | **erst mit der Klickvorlage** | Title, Description und Seitenleiste ziehen gemeinsam auf „Diamant". Vorher widerspräche das Schema der Seite |
| OXID Certified Development (>15 Entwickler) | **mit OXID zusammen** | Gleiche Seite, gleicher Zug |
| Shopware Partner (seit 2014) | **noch nicht** | Stufe offen. Ein Credential ohne Stufe hat wenig Wert und lädt zum Nachziehen mit erfundener Stufe ein. Die Shopware-Kompetenz ist bis dahin über `knowsAbout` abgedeckt |
| Awards (Shop Usability Award) | **sofort** | Das Site-Banner führt sie auf jeder Seite |

`url` bleibt bei allen Credentials weg, solange keine auflöst — siehe
[Beleglage](../befunde-partnerverzeichnisse-2026-08-19.md). Ein Link ins Leere
ist schlechter als kein Link.

Die Vorlage
[organization-kosmonaut.jsonld](./organization-kosmonaut.jsonld) ist das
**Zielbild** mit allen Credentials. Wer sie einbaut, nimmt die Zeilen nach der
Tabelle oben gestaffelt auf, nicht in einem Rutsch.

Umsetzung laut KMT nicht in PR #135 (im letzten Gate-Durchgang, ein Nachschub
würde den Durchlauf neu starten), sondern als eigener PR direkt danach.

## Vorlage — Service je Expertise-Seite

Pro Leistungsseite ein `Service`-Knoten, der auf die Organisation zeigt:

```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "commercetools Implementierung",
  "serviceType": "commercetools Agentur",
  "provider": { "@id": "https://kosmonaut.io/#organization" },
  "areaServed": { "@type": "Country", "name": "Deutschland" },
  "url": "https://kosmonaut.io/expertise/commercetools/"
}
```

`serviceType` trägt die Fokus-Suchanfrage der Seite — dieselbe, die in Titel
und H1 steht. Zuordnung je Seite in der
[Keyword-URL-Matrix](../keyword-url-matrix.md). Voraussetzung: Der
Organization-Knoten braucht ein stabiles `@id`, das der heutige Stub nicht
setzt.

## Reihenfolge

1. **main-Port terminieren** (D9). Ohne ihn wirkt keine der folgenden Stufen
   auf Prod — das ist die eigentliche Entscheidung.
2. Organization-Ausbau auf dev, gebündelt mit dem Port, damit beides in einem
   Rutsch live geht.
3. Service-Knoten auf den Karten-Seiten A1–A3, danach die übrigen
   Expertise-Seiten.
4. `Article` für den Newsroom — erst nach dem Relaunch-Briefing.

Teil der Kosmonaut-Doku · [Matrix](../keyword-url-matrix.md) ·
[Zielwerte A1–A3](../massnahmen/zielwerte-a1-a3.md)
