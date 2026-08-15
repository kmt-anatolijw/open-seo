# M5 — Abnahme

**Ziel:** Das Gesamtsystem einmal unter echten Bedingungen belegen, dann Übergabe in den
Regelbetrieb.

|              |                                                                                                |
| ------------ | ---------------------------------------------------------------------------------------------- |
| Team         | Orchestrator selbst; [DU] zeichnet gegen                                                       |
| Verfahren    | [Runbook, Schritt 7](../cloudflare-runbook.md) + [Konzept, Abschnitt 7](../seo-team-hermes.md) |
| Setzt voraus | M1–M4 fertig                                                                                   |
| Übergabe     | Regelbetrieb: laufende Pflege bei `kosmonaut-devops`, Meldungen über OpenClaw                  |

## Arbeitspakete

1. **Ende-zu-Ende auf einer echten Kundendomain:** `/seo audit` über den Router. Prüfen:
   Hermes delegiert parallel; open-seo wurde nur gerufen, wo die Routing-Tabelle es
   vorsieht; PDF-Review meldet `PASS`.
2. **Plattform-Crons belegen:** einen Rank-Tracker anlegen; innerhalb von zehn Minuten
   steht ein Messwert.
3. **Meldekette belegen:** eine Drift-Meldung — echt oder simuliert — kommt in OpenClaw
   an.
4. **Runbook-Restpunkte:** Telemetrie-Entscheidung gesetzt, Abnahmeliste aus Schritt 7
   vollständig.
5. Abnahmeprotokoll an [DU]; Gegenzeichnung einholen. Maßstab sind die Abnahmelisten
   aller Meilensteine plus die Runbook-Liste aus Schritt 7.

## Abnahme

- [ ] Audit-Lauf: parallel delegiert, Routing eingehalten, PDF `PASS`
- [ ] Rank-Tracker-Messwert nach spätestens zehn Minuten — der Beweis, dass die
      Plattform-Crons feuern
- [ ] OpenClaw hat mindestens eine echte Meldung zugestellt
- [ ] Runbook-Abnahmeliste (Schritt 7) vollständig abgehakt
- [ ] [DU] hat gegengezeichnet

Teil des [Masterplans](./README.md).
