# M2 — Hermes-Werkbank steht

**Ziel:** Hermes mit ECC (Kern, Marketing, Memory), den claude-seo-Fachskills und dem
Router-Skill — arbeitsfähig, noch ohne open-seo-Anbindung (die kommt in M3).

|                |                                                                                        |
| -------------- | -------------------------------------------------------------------------------------- |
| Team           | Hermes-Setup-Agent auf der Hermes-Maschine                                             |
| Verfahren      | [Konzept](../seo-team-hermes.md), Abschnitt 4, Schritte 1–3 und 5                      |
| Setzt voraus   | nichts — parallel zu M1                                                                |
| Übergabe an M3 | Hermes läuft und listet die Skills; `~/.hermes/config.yaml` bereit für den MCP-Eintrag |

## Arbeitspakete

1. **ECC installieren** (Konzept Schritt 1): `ecc-universal`, `ecc memory init`. Existiert
   bereits ein `~/.hermes`, zuerst den Migrationspfad fahren.
2. **Module wählen** (Schritt 2):
   `./install.sh --target hermes --profile minimal --with business-content --with skill-unified-memory`.
3. **claude-seo einziehen** (Schritt 3): Portability-Check, Skills und Agents kopieren,
   `claude-seo setup` für Venv und Chromium.
4. **Router-Skill installieren** (Schritt 5):
   [`kosmonaut-seo`](../skills/kosmonaut-seo/SKILL.md) nach `~/.hermes/skills/`.
5. **Namenskollision entschärfen:** ECCs `seo`-Skill — Beifang aus `business-content` —
   aus `~/.hermes/skills/` entfernen oder seine `description` entschärfen. Sonst
   konkurriert er mit claude-seo um dieselben Anfragen.
6. Installierte Skill-Liste an den Orchestrator melden.

## Abnahme

- [ ] `python3 scripts/portability_check.py` auf der Zielmaschine: 0 Fehler
- [ ] `command -v ecc-memory-mcp` liefert einen Pfad
- [ ] `node tests/run-all.js` im ECC-Checkout grün
- [ ] Hermes startet und listet die claude-seo-Skills und `kosmonaut-seo`
- [ ] ECCs `seo`-Skill entfernt oder entschärft

## Bekannte Fehlerbilder

| Symptom                                  | Ursache                                     | Abhilfe                                                                                     |
| ---------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Python-Scripts brechen mit Importfehlern | Script direkt mit dem Interpreter gestartet | immer `claude-seo run <script>` — Ausnahme: `portability_check.py` (nur Standardbibliothek) |
| SEO-Anfragen landen im falschen Skill    | ECCs `seo`-Skill noch aktiv                 | Arbeitspaket 5                                                                              |
| Unerwartete Kanten im Hermes-Adapter     | ECC führt Hermes als „experimental"         | Befund an den Orchestrator melden, nicht lokal umbauen                                      |

Teil des [Masterplans](./README.md).
