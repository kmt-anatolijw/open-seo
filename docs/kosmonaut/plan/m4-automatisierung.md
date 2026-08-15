# M4 — Automatisierung scharf

**Ziel:** Die drei geplanten Läufe — Drift-Wache, Monatsaudit, Credit-Wächter — laufen in
Hermes-Cron und melden über OpenClaw.

|                |                                                                                     |
| -------------- | ----------------------------------------------------------------------------------- |
| Team           | Hermes-Betriebs-Agent auf der Hermes-Maschine                                       |
| Verfahren      | [cron-jobs.md](../cron-jobs.md) — die Prompts sind vollständig, inkl. Abbruchregeln |
| Setzt voraus   | M3 — die Kundenliste bestätigt [DU] in Arbeitspaket 1                               |
| Übergabe an M5 | drei Jobs mit je einem grünen Handlauf, Zeitplan aktiv                              |

**Keinen Rank-Check-Job anlegen.** Die Rank-Checks feuern auf Cloudflare als
Plattformfunktion — cron-jobs.md erklärt es. Ein Hermes-Job dafür verdoppelt die Arbeit
und verbrennt Credits.

## Arbeitspakete

1. **Kundenliste unter `~/.hermes/workspace/seo/kundenliste.md` ablegen**; alle drei
   Prompts verweisen darauf. [DU] bestätigt die Liste.
2. **Je Domain einmalig `/seo drift baseline <domain>`** — ohne Baseline meldet die
   Drift-Wache täglich einen Fehler.
3. **Cron-Schema ermitteln:** Aufbau von `~/.hermes/cron/jobs.json` aus einer bestehenden
   Datei oder `ecc migrate audit --source ~/.hermes` ablesen — nicht raten; cron-jobs.md
   erklärt, warum dort kein fertiges Schema steht.
4. **Drei Jobs eintragen**, Prompts wörtlich aus cron-jobs.md.
5. **Jeden Job einmal von Hand auslösen** und die Sentinel-Antworten prüfen
   (`KEINE ABWEICHUNG`, `CREDITS OK`).

## Abnahme

- [ ] Drift-Wache: Handlauf grün; ohne Abweichung exakt `KEINE ABWEICHUNG`, keine Meldung
      an OpenClaw
- [ ] Monatsaudit: Handlauf für **eine** Domain grün, PDF-Review `"status": "PASS"`,
      Ablagepfad stimmt
- [ ] Credit-Wächter: Handlauf grün; ein Auth-Fehler wird als Auth-Fehler gemeldet, nicht
      als Guthabenstand
- [ ] Zeitplan aktiv: täglich 06:15, 1. des Monats 04:00, montags 08:00

## Bekannte Fehlerbilder

| Symptom                                      | Ursache                                                | Abhilfe                                                                                     |
| -------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Drift-Wache meldet täglich einen Fehler      | Baseline fehlt für mindestens eine Domain              | Arbeitspaket 2                                                                              |
| Credit-Wächter schweigt trotz leerer Credits | Access-Sitzung abgelaufen — sieht aus wie voller Stand | Prompt unverändert aus cron-jobs.md übernehmen; er unterscheidet Auth- von Guthaben-Fehlern |
| Läufe bremsen sich gegenseitig aus           | Zeiten verschoben oder Domains parallel auditiert      | versetzte Zeiten beibehalten; Monatsaudit arbeitet Domains nacheinander ab                  |

Teil des [Masterplans](./README.md).
