# OMR-Seiten: technische GEO-Prüfung

19.08.2026 · SEO-Session · Auftrag Anatolij: „Rufe mit den seo-geo-Skills sauber
die Inhalte ab und prüfe detailliert, wie wir das in unser Konzept aufnehmen."

Methode: Live-Abruf aller Seiten am 19.08.2026 ohne JavaScript (reines HTTP,
Browser-UA), Auswertung mit `parse_html.py` aus dem claude-seo-Runtime, dazu
`robots.txt`, `llms.txt` und die JSON-LD-Blöcke jeder Seite. Ohne JS zu rendern
ist Absicht: **KI-Crawler führen kein JavaScript aus.** Was im Roh-HTML fehlt,
existiert für sie nicht.

## 1. Statusprüfung — alle Seiten sind sauber ausgeliefert

| Seite | HTTP | Robots-Meta | Kosmonaut im Roh-HTML | Erste Nennung bei |
| --- | --- | --- | --- | --- |
| `contenthub/shopify-plus-best-practice-partneragenturen` | 200 | index, follow | **13×** | **6,2 %** |
| `contenthub/oxid-best-practice-partneragenturen` | 200 | index, follow | **12×** | **7,1 %** |
| `contenthub/e-commerce-agentur` | 200 | index, follow | 6× | 23,5 % |
| `contenthub/e-commerce-pim` | 200 | index, follow | 3× | 45,0 % |
| `contenthub/b2b-shop` | 200 | index, follow | 3× | 49,3 % |
| `contenthub/oxid-shop` | 200 | index, follow | 3× | 57,8 % |
| `product/shopify-plus` | 200 | index, follow | 12× | — |
| `product/oxid` | 200 | index, follow | 9× | — |
| `product/commercetools` | 200 | index, follow | 7× | — |
| `service/kosmonaut-germany` | 200 | index, follow | 63× | — |

Alle Seiten liefern `max-snippet:-1, max-image-preview:large` — OMR erlaubt
Google ausdrücklich unbegrenzte Snippet-Länge. Das ist die Voraussetzung dafür,
dass eine Passage überhaupt vollständig in einer KI-Antwort landen kann.

**Server-seitig gerendert:** Jede Kosmonaut-Nennung steht im Roh-HTML, ohne dass
ein Browser JavaScript ausführen muss. Für KI-Crawler ist der Inhalt vollständig
lesbar.

**„Erste Nennung bei"** ist die Position der ersten Kosmonaut-Erwähnung im
Artikelkorpus. Relevant, weil rund 44 % der KI-Zitationen aus dem ersten Drittel
einer Seite stammen (SE-Ranking-Studie). Die beiden Case Studies liegen bei 6
und 7 Prozent — besser geht es kaum. Die drei Ratgeberartikel liegen in der
zweiten Hälfte, wo Kosmonaut nur noch als Randempfehlung mitläuft.

## 2. Der entscheidende Befund: OMR lässt die KI-Crawler herein

Aus `omr.com/robots.txt`, wörtlich gruppiert:

```
User-agent: GPTBot          # OpenAI
User-agent: Google-Extended # Google
User-agent: anthropic-ai    # Anthropic
User-agent: Claude-Web      # Anthropic browser
User-agent: PerplexityBot   # Perplexity
User-agent: Bytespider, cohere-ai, CCBot, FacebookBot, Amazonbot, voyageai
Disallow: /*/reviews/new/
Allow: /*/reviews/
```

**Jeder große KI-Crawler darf `/reviews/` lesen — explizit erlaubt, nicht bloß
geduldet.** Damit ist die GEO-Wirkung der OMR-Platzierungen keine Vermutung: Der
Zugang ist auf Serverebene freigegeben.

Gleichzeitig blockt OMR die SEO-Crawler vollständig:

```
User-agent: SemrushBot   → Disallow: /
User-agent: AhrefsBot    → Disallow: /
User-agent: SeobilityBot → Disallow: /
```

Praktische Folge für uns: **Kennzahlen zu OMR-Seiten aus Ahrefs oder Semrush
sind unvollständig und nicht belastbar.** Wer den Wert dieser Platzierungen
messen will, misst ihn nicht über Drittanbieter-Crawler, sondern über die
eigene Search Console (Markenanfragen) und über KI-Antwort-Stichproben.

`omr.com/llms.txt` existiert (HTTP 200, 61 Zeilen), listet aber nur
URL-Schemata, keine Einzelseiten. Kein Kosmonaut-Eintrag — und das ist kein
Mangel: Google ignoriert `llms.txt` erklärtermaßen, es ist kein Hebel.

## 3. Frische — hier liegt das Problem

KI-Systeme bevorzugen aktuelle Quellen deutlich: Inhalte unter drei Monaten
werden rund dreimal häufiger zitiert, ab etwa sechs Monaten ohne Aktualisierung
sinkt die Zitierfähigkeit spürbar (SE Ranking, Auswertung von 1,3 Mio.
Zitationen).

| Seite | veröffentlicht | zuletzt aktualisiert | Alter heute |
| --- | --- | --- | --- |
| E-Commerce-Agentur (Top-10-Liste) | 20.02.2025 | **13.05.2026** | 3,2 Monate ✅ |
| E-Commerce-PIM | 25.02.2022 | 09.01.2026 | 7,3 Monate ⚠️ |
| **Shopify-Plus-Case-Study (Kosmonaut)** | 21.02.2025 | 10.11.2025 | **9,3 Monate** ⚠️ |
| **OXID-Case-Study (Kosmonaut)** | 15.04.2025 | 10.11.2025 | **9,3 Monate** ⚠️ |
| OXID-Shop | 11.08.2025 | 25.08.2025 | 11,8 Monate ❌ |
| B2B-Shop | 15.04.2025 | 23.04.2025 | **15,9 Monate** ❌ |

Die beiden Seiten, die Kosmonaut am stärksten tragen, sind neun Monate nicht
angefasst worden. OMR pflegt seine Artikel nachweislich — zwei der sechs haben
ein Aktualisierungsdatum aus 2026. Es gibt also einen Redaktionsprozess, in den
man hineinkommen kann.

## 4. Was in den Artikeln tatsächlich steht

### Zwei vollwertige Case Studies über Kosmonaut

Das ist mehr, als bisher in der [OMR-Strategie](./omr-strategie-2026-08-19.md)
stand. Es sind keine Listeneinträge, sondern redaktionelle Fallstudien:

**Shopify Plus / Ludwig von Kapff** (Autorin Chantal Seiter, als „Sponsored"
gekennzeichnet). Die Meta-Description nennt Kosmonaut namentlich:

> „So hat die Agentur KOSMONAUT den Weinhändler Ludwig von Kapff bei der
> Migration seines Onlineshops zu Shopify Plus unterstützt."

Inhaltlich belegt: Wechsel von Magento 1.9 auf Shopify Plus, Replatforming als
benannte Leistung, dazu Makaira, OXID und Shopware als weitere Systeme.

**OXID / Lucky Bike** (dieselbe Autorin). Enthält ein wörtliches Zitat:

> „Die Anforderungen an Skalierbarkeit, Performance und Integration neuer
> Funktionen mussten dringend auf ein neues Level gehoben werden", sagt
> **Evgenij Bazenov, COO der KOSMONAUT Germany GmbH**.

Ein namentlich zugeordnetes Zitat einer Führungskraft auf einer Domain mit
Autoritätswert 82 ist das stärkste Einzelsignal im ganzen OMR-Bestand — es
verknüpft eine Person mit der Firma und einem Fachgebiet.

### Platz 3 von 10 im Agenturvergleich

Im Artikel „Zehn E-Commerce-Agenturen im Vergleich" steht Kosmonaut auf Position
drei, mit eigenem Absatz, Standortangabe und Leistungsprofil. Erste Nennung bei
23,5 Prozent der Seite — noch im zitierfähigen ersten Drittel.

### Zwei Empfehlungssätze in Ratgebern

In `e-commerce-pim` und `b2b-shop` steht jeweils ein Satz der Machart
„Die E-Commerce-Agentur KOSMONAUT Germany kann dich dabei unterstützen …",
und Kosmonaut ist auf beiden Seiten der **einzige** verlinkte Dienstleister.
Beide Sätze stehen allerdings in der zweiten Seitenhälfte.

## 5. Zwei Lücken, die niemand bisher benannt hat

### Kein einziger Link auf kosmonaut.io

Geprüft über alle sechs Content-Hub-Seiten: **null** Verweise auf die eigene
Domain. Die Artikel verlinken ausschließlich auf das OMR-interne Profil. Der
einzige externe Link steht auf der Profilseite selbst — und trägt
`rel="nofollow"`.

Das ändert nichts am Wert, verschiebt aber die Begründung: OMR liefert **keine
Linkkraft**. Der gesamte Nutzen liegt in Erwähnungs- und Entitätssignalen. Das
ist kein Nachteil — Markenerwähnungen korrelieren rund dreimal stärker mit
KI-Zitationen als Backlinks (Ahrefs, 75.000 Marken) — aber es heißt: Wer diese
Platzierungen als Linkbuilding verkauft, misst das Falsche.

### Die stärksten Assets gibt es nur auf Deutsch

| Seite | deutsche Fassung | englische Fassung |
| --- | --- | --- |
| Shopify-Plus-Case-Study | ✅ | **404** |
| OXID-Case-Study | ✅ | **404** |
| E-Commerce-Agentur (Top 10) | ✅ | **404** |
| E-Commerce-PIM | ✅ | ✅ (7 Nennungen) |
| B2B-Shop | ✅ | ✅ (7 Nennungen) |
| OXID-Shop | ✅ | 404 |

Beide Case Studies und der Agenturvergleich — also alles, was Kosmonaut
substanziell beschreibt — existieren ausschließlich auf Deutsch. Für ein
Unternehmen mit Standorten in Sofia und Ho-Chi-Minh-Stadt und internationalem
Anspruch ist das eine Lücke, die OMR schließen könnte: Für zwei der sechs
Seiten macht die Redaktion es bereits.

## 6. Ein Widerspruch, der aufgelöst werden muss

Die OXID-Case-Study nennt Kosmonaut „zertifizierter **OXID Enterprise Solution
Partner**". Das Organisationsschema, das wir vorbereitet haben, weist
„**Diamant-Partner** seit 2025" aus
([organization-kosmonaut.jsonld](./schema/organization-kosmonaut.jsonld)).

Beides war zu seiner Zeit richtig — der Artikel ist von April 2025, der
Diamant-Status kam später. Für die Entitätserkennung ist ein solcher
Widerspruch aber genau die Art Signal, die Vertrauen kostet: Zwei Quellen sagen
Unterschiedliches über dieselbe Firma.

**Das ist zugleich der beste Anlass für den Refresh.** Eine Redaktion
aktualisiert lieber, wenn es einen sachlichen Grund gibt, und „unser
Partnerstatus hat sich geändert" ist genau das.

## 7. Was daraus folgt

Vier Punkte, alle mit Beleg aus dieser Prüfung:

1. **Refresh der beiden Case Studies anfragen** — mit dem Diamant-Status als
   Anlass, dazu 80+ OXID-Projekte, 70+ API-Projekte und der Data Hub. Das hebt
   zwei Seiten von 9,3 Monaten auf frisch und räumt den Widerspruch aus.
2. **Englische Fassungen der Case Studies anfragen.** OMR macht es für zwei der
   sechs Seiten bereits; die Anfrage ist damit keine Sonderbitte.
3. **Frühere Platzierung in den drei Ratgebern anfragen.** Von 45–58 Prozent
   Seitentiefe ins erste Drittel — dorthin, wo die Zitationen entstehen.
4. **Den Bewertungsaufbau nicht als Linkbuilding rechnen.** `nofollow` und keine
   Artikel-Links: Der Wert ist Entität und Erwähnung, und der zählt genau dort,
   wo Kosmonaut hinwill — in KI-Antworten.

Die Reihenfolge und der Aufwand stehen in der
[OMR-Strategie](./omr-strategie-2026-08-19.md), Abschnitt 4.

## Nicht geprüft

- **Status über den OpenSEO-MCP:** Der Server ist in dieser Session nicht
  verbunden (`claude mcp list` listet ihn nicht), und MCP-Server hinzufügen ist
  nichts, was ich eigenmächtig tue. Die Statusprüfung oben stammt aus
  Live-Abrufen und dem claude-seo-Runtime.
- **Tatsächliche KI-Zitationen:** Ob und wie oft omr.com in Antworten von
  ChatGPT, Perplexity oder Google AI Mode auftaucht, ist hier nicht gemessen.
  Belegt ist der Zugang (robots.txt), nicht die Zitation.

Teil der Kosmonaut-Doku · [OMR-Strategie](./omr-strategie-2026-08-19.md) ·
[Schema](./schema/README.md) · [Data-Hub-Briefing](./relaunch-briefing-data-hub.md)
