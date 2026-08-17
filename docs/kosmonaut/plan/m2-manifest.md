# M2 — Installationsmanifest (Plan-Gate-Eingabe)

Deterministische Soll-Liste für die Abnahme „Hermes listet die Skills".
Stand 15.08.2026, erstellt für das Plan-Gate der Hermes-Config-Session.

## Quellen (gepinnt)

| Komponente | Quelle | Pin |
| --- | --- | --- |
| claude-seo | `kmt-anatolijw/claude-seo` (lokal `~/www/hermes/claude-seo`) | Commit `f9b28edbc5c3162f4d269772d423103c24bf4848` (= Upstream-Release v2.2.4 + 1 Doku-Commit) |
| ECC | `affaan-m/ECC` | Tag `v2.1.0`, Commit `4da6deac1888` — identisch mit npm `ecc-universal@2.1.0` (dist-tag `latest`) |
| Router-Skill | openseo-Repo, `docs/kosmonaut/skills/kosmonaut-seo/` | mit dem jeweiligen openseo-Checkout |

> **Prüfauftrag ans Plan-Gate:** Die Hermes-Adapter-Doku von ECC nannte zuletzt
> v2.0.0-rc.1. Vor dem Build verifizieren, dass der Hermes-Pfad
> (`docs/HERMES-SETUP.md`, `scripts/install-apply.js`, `--target hermes`,
> `--without`-Parsing) im Tag v2.1.0 vorhanden und funktionsfähig ist.

## Skills — Soll: 26

25 aus `claude-seo/skills/` plus der Router:

```
seo            seo-audit         seo-backlinks   seo-cluster    seo-competitor-pages
seo-content    seo-content-brief seo-dataforseo  seo-drift      seo-ecommerce
seo-flow       seo-geo           seo-google      seo-hreflang   seo-image-gen
seo-images     seo-local         seo-maps        seo-page       seo-plan
seo-programmatic seo-schema      seo-sitemap     seo-sxo        seo-technical
kosmonaut-seo  (Router, aus openseo)
```

Skills mit Unterverzeichnissen (`references/`, `templates/`) werden vollständig
kopiert: `seo`, `seo-google`, `seo-cluster`, `seo-sxo`, `seo-drift`,
`seo-ecommerce`, `seo-image-gen`.

## Agents — Soll: 18

Aus `claude-seo/agents/`:

```
seo-backlinks  seo-cluster  seo-content      seo-dataforseo  seo-drift
seo-ecommerce  seo-flow     seo-geo          seo-google      seo-image-gen
seo-local      seo-maps     seo-performance  seo-schema      seo-sitemap
seo-sxo        seo-technical seo-visual
```

Zielpfad: den gültigen Subagent-Pfad der laufenden Hermes-Version vor dem Build
verifizieren (Review-Befund HIGH-5 in [m2-hermes.md](./m2-hermes.md)).

## Ausdrücklich NICHT installieren

| Was | Warum |
| --- | --- |
| `claude-seo/extensions/*/skills/` (8 SKILL.md: seo-ahrefs, seo-bing, seo-dataforseo-Mirror, seo-firecrawl, seo-image-gen-Mirror, seo-profound, seo-seranking, seo-unlighthouse) | Install-Helper für externe MCP-Server, die in Hermes nicht existieren. Erklärt die Zählung 33 vs. 25 aus dem portability_check. |
| ECC-Skill `seo` (Beifang aus `business-content`) | Namenskollision mit claude-seo — entfällt by design: die `--skills`-Form (unten) installiert den ECC-`seo`-Pfad gar nicht erst. |

## ECC-Module

**Korrigiert 17.08.2026** (Dry-Run-Befund der Hermes-Config-Session gegen
v2.1.0): `--with`/`--without` nehmen Component-IDs, keine Modul-IDs
(`Error: Unknown install component: business-content`), und das Modul
`business-content` wird auf `--target hermes` vom Resolver **still** geskippt
(targets-Liste ohne hermes). Die ursprüngliche Zeile war damit doppelt
unwirksam. Verifizierte Form:

```
--profile minimal --target hermes \
--skills article-writing,brand-voice,content-engine,investor-materials,\
investor-outreach,lead-intelligence,product-capability,social-graph-ranker,\
market-research,brand-discovery,competitive-platform-analysis,\
competitive-report-structure,marketing-campaign
```

(= business-content-Pfade außer `seo`; `skill-unified-memory` ist im
minimal-Profil bereits enthalten, ein separates `--with` entfällt.
**Zählabgleich im Plan-Gate:** die Liste oben hat 13 Einträge, der Befund
sprach von „14 Pfaden außer seo" — gegen `manifests/install-modules.json`
abgleichen, ob ein Pfad fehlt oder 14 = 13 + seo gemeint war.)

## Abnahme-Zählwerte

- Hermes listet **26 claude-seo-Skills** (Liste oben, exakt) und
  **18 claude-seo-Agents** (exakt); die ECC-Basis (minimal-Profil installiert
  via agents-core ~157 eigene Agent-Dateien) wird separat ausgewiesen und
  zählt nicht gegen dieses Soll.
- Kein Skill namens `seo` aus ECC-Quelle vorhanden.
- Zählung nach Container-Recreate unverändert.

Teil des [Masterplans](./README.md); Korrekturen und Host-Rahmen in
[m2-hermes.md](./m2-hermes.md).
