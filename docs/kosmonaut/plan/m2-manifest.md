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
| ECC-Skill `seo` (Beifang aus `business-content`) | Namenskollision mit claude-seo; bevorzugt via `--without seo` gar nicht erst installieren. |

## ECC-Module

`--target hermes --profile minimal --with business-content --with skill-unified-memory`
(minus `seo`, siehe oben).

## Abnahme-Zählwerte

- Hermes listet **26 Skills** (Liste oben, exakt) und **18 Agents**.
- Kein Skill namens `seo` aus ECC-Quelle vorhanden.
- Zählung nach Container-Recreate unverändert.

Teil des [Masterplans](./README.md); Korrekturen und Host-Rahmen in
[m2-hermes.md](./m2-hermes.md).
