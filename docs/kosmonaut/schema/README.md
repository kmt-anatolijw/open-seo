# Strukturierte Daten kosmonaut.io — Befund und Vorlagen

Stand 19.08.2026 · SEO-Session

## Befund: die Domain liefert keinerlei strukturierte Daten

Zweifach verifiziert am 19.08.2026:

| Prüfweg | Seiten | Ergebnis |
| --- | --- | --- |
| Roh-HTML (`fetch_page.py`) | Startseite, `/expertise/commercetools/`, `/expertise/oxid-shop/`, `/newsroom/insights/apisunverzichtbar/` | **0** `application/ld+json`-Blöcke |
| Gerendertes DOM (`render_page.py`, Playwright, `is_spa=true`, 4,9 s Renderzeit) | `/expertise/commercetools/` | **0** Vorkommen |

Damit ist auch die Annahme aus dem Council-Addendum (Punkt 5) korrigiert:
`structured-data.ts` mag Organization/WebSite/BreadcrumbList erzeugen — **ausgeliefert
wird davon nichts.** Der Code ist offenbar nicht eingebunden. Das ist ein
Einbau-Befund für die KMT-Session, kein Neuentwurf.

## Warum das teuer ist

1. **Die Partnerstatus sind maschinell unsichtbar.** OXID Diamant-Partner,
   commercetools Solution Partner, Shopify Plus Partner und der Shop Usability
   Award stehen nur als Fließtext auf der Seite. Weder Google noch die
   AI-Systeme können sie einer Entität zuordnen.
2. **Kein Entitäts-Anker.** Ohne `Organization`-Knoten mit `sameAs` fehlt die
   Verbindung zwischen der Website, dem LinkedIn-Profil und dem
   commercetools-Partnerverzeichnis. Das ist die Grundlage für
   Knowledge-Panel und für Zitate in KI-Antworten.
3. **Kein Breadcrumb in der Trefferliste.** Bei einer Seitenstruktur mit
   `/expertise/`, `/services/`, `/projekte/` und `/newsroom/` verschenkt das
   Kontext in jedem einzelnen Treffer.

Die Domain hat in 90 Tagen 38.962 nicht-markenbezogene Einblendungen und
daraus 10 Klicks gezogen. Strukturierte Daten sind nicht die Ursache — aber
sie sind der billigste Hebel auf der Darstellungsseite, weil sie einmal
eingebaut für alle Seiten wirken.

## Vorlage 1 — Organization (einmal, global)

[organization-kosmonaut.jsonld](./organization-kosmonaut.jsonld) — fertig zum
Einbau in das Layout, gilt für jede Seite.

Belegt durch: Impressum (Firmierung, Anschrift, USt-IdNr, Geschäftsführung),
Site-Banner (Awards), `/newsroom/news/commercetools/` und
`commercetools.com/partners/kosmonaut` (Solution Partner),
`/newsroom/news/oxid-zertifizierung/` (Certified Development),
Anatolij 19.08.2026 (OXID Diamant-Stufe), D3-Freigabe 17.08.2026 (Shopify Plus).

**Bewusst nicht enthalten:** die Shopware-Partnerstufe. Shopware führt seit
2026 Bronze/Silver/Gold/Platinum; welche Stufe gilt, ist offen. Lieber leer
als falsch — ergänzen, sobald bestätigt.

**Nicht enthalten, weil ungeprüft:** die Zahl der zertifizierten
Entwicklerinnen und Entwickler. Die Meldung von 2020 nennt zehn; Anatolij
sagt, es sind inzwischen mehr. Sobald die Zahl feststeht, gehört sie in die
`description` der Certified-Development-Credential — eine Zahl ist in der
Trefferliste und in KI-Antworten stärker als ein Adjektiv.

## Vorlage 2 — Service je Expertise-Seite (Muster)

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
und H1 steht. Die Zuordnung je Seite steht in der
[Keyword-URL-Matrix](../keyword-url-matrix.md).

## Vorlage 3 — BreadcrumbList

Aus dem vorhandenen `BreadcrumbText`-Feld der Seiten ableitbar (die Komponenten
tragen es bereits, siehe Live-HTML: `"BreadcrumbText":"Newsroom / News"`).
Kein neues Redaktionsfeld nötig.

## Reihenfolge

1. Organization global einbauen — wirkt sofort auf allen Seiten, ein Deploy.
2. BreadcrumbList — nutzt vorhandene Daten.
3. Service auf den drei Karten-Seiten A1–A3, danach auf den übrigen
   Expertise-Seiten.
4. `Article` für den Newsroom — erst nach dem Relaunch-Briefing, weil dort
   ohnehin über die Artikelstruktur entschieden wird.

Teil der Kosmonaut-Doku · [Matrix](../keyword-url-matrix.md) ·
[Zielwerte A1–A3](../massnahmen/zielwerte-a1-a3.md)
