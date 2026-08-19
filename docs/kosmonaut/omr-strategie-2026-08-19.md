# OMR Reviews — Bestandsaufnahme und Plan für Trust und KI-Sichtbarkeit

19.08.2026 · SEO-Session · Auftrag Anatolij: „Analysiere detailliert und plane,
wie wir den OMR-Stuff für uns ausnutzen können, insbesondere Trust und GEO."

Datenbasis: Live-Abrufe der sieben genannten OMR-Seiten am 19.08.2026 (HTTP
200, teils per Playwright gerendert), Ahrefs Domain Rating, Vergleichsabrufe
von vier Wettbewerberprofilen.

## Kurzfassung

Kosmonaut steht auf OMR deutlich besser da als auf der eigenen Website — und
nutzt es nicht. Auf drei Seiten ist Kosmonaut der **einzige** gelistete
Dienstleister, auf einer davon ausdrücklich als „OMR Exclusive Partner".
Gleichzeitig hat das eigene Profil **null selbst eingeworbene Bewertungen**,
während vergleichbare Agenturen zwischen 18 und 75 vorweisen. Die Plattform
hat Domain Rating 82, kosmonaut.io hat 32.

Das ist die günstigste Trust- und KI-Sichtbarkeits-Maßnahme im gesamten
Projekt: Die Platzierungen sind bereits vergeben, es fehlt nur die Substanz
dahinter.

## 1. Was Kosmonaut auf OMR heute besitzt

| Seite | Position | Mitbewerber auf derselben Seite |
| --- | --- | --- |
| **Shopify Plus** (Produktseite) | **einziger Partner**, ausgewiesen als „OMR Exclusive Partner" | keine |
| **B2B Shop** (Content Hub) | **einziger verlinkter Dienstleister** | keine |
| **E-Commerce PIM** (Content Hub) | **einziger verlinkter Dienstleister** | keine |
| **OXID** (Produktseite) | 1 von 2 im „trusted partner network" | dixeno |
| **commercetools** (Produktseite) | 1 von 4 | foryouandyourcustomers, piazza-blu, webmatch |
| **„Zehn E-Commerce-Agenturen im Vergleich"** (Content Hub, DE) | 1 von 11 gelisteten Agenturen | antiloop, basecom, basilicom, blackbit, brandung, elbkapitaene, netformic, onmacon, saphir-solution, tante-e |

Auf der Shopify-Plus-Seite steht neben dem Namen die Ortsangabe „Remote ·
Bielefeld · Osnabrück · Sofia · Ho Chi Minh City" — mehr Standorte, als die
eigene Website nennt.

## 2. Die Lücke: das Profil trägt nichts

Gemessen am 19.08.2026 auf `omr.com/en/reviews/service/kosmonaut-germany`:

| | KOSMONAUT | basecom | brandung | blackbit | dixeno |
| --- | --- | --- | --- | --- | --- |
| Bewertungen | **7** | 75 | 40 | 33 | 18 |
| Schnitt | **5,0** | 4,6 | 4,3 | 4,6 | 4,3 |
| davon selbst eingeworben | **0** | — | — | — | — |

Das Profil ist als „managed by the owner (0 reviews)" ausgewiesen: Die sieben
Bewertungen stammen laut OMR „in part or in full from external sources", nicht
aus eigener Einwerbung. **Der beste Schnitt im Feld — auf der schmalsten
Basis.** In einem Vergleich von zehn Agenturen liest sich 5,0 bei 7 Stimmen
schwächer als 4,6 bei 75.

## 3. Warum das für KI-Sichtbarkeit zählt

**Die Bewertung ist bereits maschinenlesbar.** Das OMR-Profil liefert
`ProfessionalService`-Markup mit `AggregateRating`:

```json
{"@type": "AggregateRating", "ratingValue": "5.00", "reviewCount": 7,
 "bestRating": 5, "worstRating": 0}
```

Das ist genau die Struktur, die Google und die KI-Systeme auslesen — und sie
steht auf einer Domain mit Rating 82, während kosmonaut.io bei 32 liegt. Für
eine Frage wie „welche E-Commerce-Agentur für Shopify Plus" ist OMR die
autoritativere Quelle als die eigene Website, und zwar mit großem Abstand.

**Das Seitenformat ist LLM-Futter.** „Zehn E-Commerce-Agenturen im Vergleich"
ist exakt die Art Seite, die ein Sprachmodell heranzieht, wenn jemand nach
einer Empfehlung fragt. Wer dort mit dünner Bewertungsbasis steht, wird
genannt — aber nicht empfohlen.

**Der Bedarf ist belegt.** In den eigenen Search-Console-Daten stehen bereits
Fragen in natürlicher Sprache: „welche agentur ist spezialist für headless
commerce in deutschland?" (35 Impressionen, Position 9,9), „welche agentur
hilft mir bei einer headless e-commerce entwicklung?" (Position 10,6). Solche
Formulierungen werden genauso an Chatbots gestellt.

*Nicht gemessen:* ob und wie oft omr.com konkret in KI-Antworten zitiert wird.
Das erfordert einen eingerichteten Brand-Radar-Report bei Ahrefs, den es für
dieses Konto nicht gibt. Die Argumentation stützt sich auf Autorität,
Markup und Seitenformat, nicht auf gemessene Zitationen.

## 4. Der Plan

### Stufe 1 — sofort, ohne Vorbedingung

1. **Profil beanspruchen.** Solange „managed by the owner (0 reviews)" steht,
   ist jede weitere Maßnahme wirkungslos. Das ist die Voraussetzung für
   Pflege, Bewertungseinladungen und Aktualisierungen.
2. **Standortangaben vereinheitlichen.** OMR nennt Bielefeld, Osnabrück, Sofia
   und Ho Chi Minh City, das Impressum Rheda-Wiedenbrück. Für die
   Entitätserkennung — Google wie KI — muss dieselbe Firma überall dieselbe
   Adresse tragen. Nebenstandorte gehören dazu, aber der Hauptsitz muss
   übereinstimmen.
3. **Profil-URL verlinken.** Von der eigenen Website auf das OMR-Profil, und
   das Profil in `sameAs` des Organisations-Schemas aufnehmen (bereits
   eingetragen in [organization-kosmonaut.jsonld](./schema/organization-kosmonaut.jsonld)).
   Das verbindet die beiden Entitäten in beide Richtungen.

### Stufe 2 — Bewertungen einwerben

Ziel: **von 7 auf mindestens 25 Bewertungen** — dann steht Kosmonaut im
Vergleichsfeld nicht mehr am unteren Rand. Bei einem Schnitt von 5,0 ist jede
weitere Bewertung reiner Zugewinn.

Vorgehen: Bestandskunden aus abgeschlossenen Projekten anschreiben. Die
Referenzen der Website sind die naheliegende Liste. Wichtig ist die
Gleichverteilung über die Systeme — Bewertungen, die OXID, commercetools,
Shopware und Shopify Plus abdecken, stützen alle vier Produktseiten, auf
denen Kosmonaut gelistet ist.

Kein Anreiz, keine Vorformulierung: OMR prüft Bewertungen, und ein Muster
gleichlautender Texte schadet mehr als es nützt.

### Stufe 3 — die Exklusivpositionen ausbauen

Auf drei Seiten ist Kosmonaut der einzige Dienstleister. Das ist eine
Position, die andere Agenturen aktiv anstreben — sie sollte belegt und
sichtbar gemacht werden:

- **Shopify Plus:** Die Bezeichnung „OMR Exclusive Partner" ist ein
  Vertrauenssignal, das auf der eigenen Shopify-Seite nirgends auftaucht.
  Gehört in den Nachweisblock, den der Content-Plan für Woche 4 ohnehin
  vorsieht.
- **B2B Shop und E-Commerce PIM:** Beide Content Hubs sind thematisch
  besetzt, ohne dass die eigene Website entsprechende Seiten hätte. Das ist
  ein Hinweis auf zwei Themen, für die Kosmonaut extern bereits als Adresse
  gilt — Kandidaten fürs Relaunch-Briefing.

### Stufe 4 — Inhalte in die Content Hubs

OMR-Content-Hubs zitieren Fachleute und verweisen auf Dienstleister. Wer dort
inhaltlich beiträgt, verstetigt die Platzierung. Das ist Redaktionsarbeit mit
längerer Laufzeit und gehört nicht ins 60-Tage-Fenster, sondern in die
Planung danach.

## 5. Aufwand und Erwartung

| Maßnahme | Aufwand | Wirkung |
| --- | --- | --- |
| Profil beanspruchen | 15 Minuten | Voraussetzung für alles Weitere |
| Standorte vereinheitlichen | 30 Minuten | Entitätserkennung, Local-Signale |
| Verlinkung beidseitig | im Schema erledigt, Website eine Zeile | Entitätsverbindung |
| 18 zusätzliche Bewertungen | Vertriebsaufgabe über Wochen | Aus dem unteren Feld heraus; wirkt auf allen sechs OMR-Seiten gleichzeitig |
| Exklusivpositionen ausweisen | Teil des ohnehin geplanten Nachweisblocks | Vertrauenssignal in der eigenen Trefferliste |

Die ersten drei Punkte kosten zusammen etwa eine Stunde und sind unabhängig
von allem anderen im Projekt. Sie hängen weder am main-Port noch an der
Klickvorlage.

## 6. Messung

- Bewertungszahl und Schnitt im Profil, monatlich.
- Position im Vergleichsartikel „Zehn E-Commerce-Agenturen im Vergleich".
- Ob das OMR-Profil bei Marken-Suchen nach „Kosmonaut" in den Ergebnissen
  auftaucht — messbar über die Search Console als zusätzliche Sichtbarkeit
  neben der eigenen Domain.
- Für KI-Zitationen wäre ein Brand-Radar-Report nötig; ob der eingerichtet
  wird, ist eine eigene Entscheidung.

Teil der Kosmonaut-Doku · [Schema](./schema/README.md) ·
[Matrix](./keyword-url-matrix.md) · [H2-Kandidaten](./massnahmen/h2-kandidaten-2026-08-19.md)
