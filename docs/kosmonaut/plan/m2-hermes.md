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

## Review-Befunde vor Ausführung (15.08.2026)

Codex-Adversarial-Review des Plans plus Host-Fakten aus der Hermes-Config-Session.
Die Arbeitspakete oben gelten nur noch mit diesen Korrekturen.

**Host-Realität (deploy/docker-compose.yml, Commit 50c2fa8):**

- Hermes läuft als **externes, digest-gepinntes Upstream-Image**
  (`nousresearch/hermes-agent@sha256:3db34ce…`), kein eigener Build.
- Persistenz: **genau ein Volume** `hermes-agent-data:/opt/data`. Alles außerhalb
  ist beim Container-Recreate weg.
- Runtime-User/HOME sind im Compose nicht gesetzt — kommen aus dem Upstream-Image,
  vor der Installation per `docker image inspect` ermitteln.
- **Egress-Proxy erzwungen:** `HTTPS_PROXY`/`HTTP_PROXY` fest auf
  `hermes-egress-proxy:8080` mit Ziel-Allowlist. `registry.npmjs.org`, PyPI und der
  Chromium-Download sind ohne explizite Freigabe blockiert — eine
  Laufzeitinstallation scheitert schon am Egress-Filter, nicht erst am Recreate.

**Konsequenzen für die Arbeitspakete:**

1. **Build-Zeit statt Laufzeit:** ECC-CLI (fixe Version) und claude-seo-Venv/Chromium
   gehören in ein vom gepinnten Image abgeleitetes Image; nur Config, Skills und
   Memory in persistente Volumes. Abgeleitetes Image = Fork eines fremden gepinnten
   Images mit eigenem Build-/Update-Pfad (bei Upstream-Releases: Digest aktualisieren,
   Image neu bauen). **Entschieden 15.08.2026 (User): abgeleitetes Image — genehmigt.**
   Build auf einer Maschine mit freiem Internet (lokal/CI); die Egress-Allowlist im
   Betrieb wird dafür nicht aufgeweicht.
2. **ECC-Checkout ist Voraussetzung:** `./install.sh` und `node tests/run-all.js`
   brauchen ein gepinntes ECC-Repo (`affaan-m/ECC`, Commit/Tag festlegen).
   „Setzt voraus: nichts" stimmt nicht; `npm install -g ecc-universal` liefert den
   Checkout nicht.
3. **ECC-Memory integrieren, nicht nur installieren:** eigener `ecc-memory-mcp`-Prozess
   mit eigener `ECC_MEMORY_HARNESS`-Identität, registriert in `~/.hermes/config.yaml`;
   `ecc memory init --scope project` aus explizit benanntem Projektverzeichnis.
4. **Agent-Pfad verifizieren:** gültigen Subagent-Pfad der laufenden Hermes-Version
   ermitteln, sonst ist die Abnahme grün, ohne dass `/seo audit` delegieren kann.
5. **ECC-`seo`-Skill gar nicht erst installieren:** prüfen, ob `install-apply.js` im
   gepinnten Stand `--without seo` unterstützt; nur sonst nachträglich deterministisch
   entfernen (keine freien `description`-Edits — lokaler Drift).
6. **Abnahme zweistufig:** Build-Kontext (portability_check, ECC-Tests) getrennt von
   Runtime-Gates im finalen Container als Runtime-User; danach Container-Recreate und
   Runtime-Gates wiederholen (Persistenzbeweis). Router-Routen nach open-seo müssen
   bis M3 sauber fail-closed enden.
7. **Skill-Manifest festlegen:** Plan sagt 25 Skills, portability_check zählt
   33 SKILL.md (Extension-Mirrors). Vor der Abnahme das Soll-Manifest fixieren.

**Prozess:** M2 wird von der Hermes-Config-Session über deren GSD-Flow ausgeführt
(eigene Phase, Plan-Gate mit Cross-AI-Review, Build-Gate mit hermes-security) —
nicht als direkte Installation aus dieser Session. Start erst nach Freigabe des
Users in der Hermes-Session; aktuell laufen dort die Build-Gates für Phase 04.1.

Teil des [Masterplans](./README.md).
