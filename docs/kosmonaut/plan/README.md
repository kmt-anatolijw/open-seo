# Masterplan: Kosmonaut SEO-Team

Zerlegt die Umsetzung von [seo-team-hermes.md](../seo-team-hermes.md) in fünf Meilensteine,
die eigenständige Agenten-Teams abarbeiten. Jeder Teilplan ist ein vollständiger
Arbeitsauftrag: Ziel, Team, Voraussetzungen, Arbeitspakete, Abnahme, bekannte Fehlerbilder.
Hintergrund und Begriffe — Hermes, ECC, OpenClaw, DataForSEO-Credits — erklärt das Konzept.

## Orchestrierung

Ein Orchestrator — eine Claude-Session mit diesem Repo — führt:

1. **Vergeben.** Je Meilenstein ein eigenständiges Team mit frischem Kontext. Das Team
   bekommt genau seinen Teilplan und die darin verlinkten Dokumente, nicht die ganze
   Historie. Beim Vergeben nennt der Orchestrator die Zielmaschine und den Zugang.
2. **[DU]-Schritte bündeln.** Alles, was nur ihr könnt — Konten, Anmeldungen im Browser,
   Schlüssel — sammelt der Orchestrator vor dem Start des Meilensteins ein. Übersicht
   unten.
3. **Abnehmen.** Ein Meilenstein ist fertig, wenn jede Zeile seiner Abnahme belegt ist.
   Erst dann gibt der Orchestrator abhängige Meilensteine frei.
4. **Blocker entscheiden.** Teams melden Blocker mit Befund; der Orchestrator entscheidet
   oder eskaliert an euch. Teams erweitern ihren Auftrag nicht selbst.

Die Teams arbeiten dort, wo die Systeme leben: der Deploy auf der Maschine mit dem
open-seo-Checkout, die Hermes-Einrichtung auf der Hermes-Maschine. Der Orchestrator
koordiniert und prüft — er ersetzt keinen der beiden Orte.

## Reihenfolge

```
M1 open-seo läuft ──┐
                    ├── M3 Verbindung ── M4 Automatisierung ── M5 Abnahme
M2 Hermes-Werkbank ─┘
```

M1 und M2 sind unabhängig und laufen parallel. M3 braucht beide, M4 braucht M3, M5 nimmt
das Ganze ab.

| Meilenstein                      | Teilplan                                         | Team                | Setzt voraus | Status              |
| -------------------------------- | ------------------------------------------------ | ------------------- | ------------ | ------------------- |
| M1 open-seo läuft auf Cloudflare | [m1-cloudflare.md](./m1-cloudflare.md)           | kosmonaut-devops    | —            | fertig (15.08.2026) |
| M2 Hermes-Werkbank steht         | [m2-hermes.md](./m2-hermes.md)                   | Hermes-Setup        | —            | offen               |
| M3 Verbindung steht              | [m3-verbindung.md](./m3-verbindung.md)           | kosmonaut-devops    | M1 + M2      | offen               |
| M4 Automatisierung scharf        | [m4-automatisierung.md](./m4-automatisierung.md) | Hermes-Betrieb      | M3           | offen               |
| M5 Abnahme                       | [m5-abnahme.md](./m5-abnahme.md)                 | Orchestrator + [DU] | M1–M4        | offen               |

Den Status pflegt der Orchestrator in dieser Tabelle: offen → läuft → fertig, mit Datum.
Übergabewerte — etwa den Worker-Hostname aus M1 — hält er beim Abhaken ebenfalls hier fest.

Übergabewerte aus M1: Worker-Hostname `open-seo-selfhost.kosmonaut-account.workers.dev`,
Zero-Trust-Team `kmt-base.cloudflareaccess.com`, Access-App `open-seo-selfhost.kosmonaut-account.workers.dev`
(Policy `SelfHostAllow`, handverwaltet — E-Mail-Allowlist lebt in dieser Policy, nicht in `ACCESS_ALLOWED_EMAILS`).

## Eure Schritte auf einen Blick

| Wann   | Was                                                                                 | Meilenstein |
| ------ | ----------------------------------------------------------------------------------- | ----------- |
| vor M1 | Cloudflare-Konto mit aktiviertem R2 (verlangt Zahlungsmethode, auch im Free Tier)   | M1          |
| vor M1 | DataForSEO-Konto anlegen, Schlüssel bereithalten                                    | M1          |
| in M1  | `alchemy login` + `bootstrap` im Browser bestätigen; beide Werte in `.env.selfhost` | M1          |
| in M3  | Managed OAuth im Zero-Trust-Dashboard einschalten, Redirect-URIs freigeben          | M3          |
| in M3  | einmalige interaktive MCP-Anmeldung auf der Hermes-Maschine                         | M3          |
| in M4  | Kundenliste bestätigen: welche Domains überwacht werden                             | M4          |
| in M5  | Abnahme gegenzeichnen                                                               | M5          |

## Grundregeln für alle Teams

- Verfahren stehen in den referenzierten Dokumenten — [Runbook](../cloudflare-runbook.md),
  [Konzept](../seo-team-hermes.md), [cron-jobs.md](../cron-jobs.md). Die Teilpläne
  wiederholen sie nicht. Weicht die Realität vom Dokument ab, gilt die Realität; das Team
  zieht das Dokument nach und informiert den Orchestrator.
- Secrets bleiben bei euch. Teams schreiben Vorlagen mit Platzhaltern, tragen nie echte
  Schlüssel ein und zitieren keine `.env`-Inhalte.
- Destruktives — `alchemy destroy`, Löschen von Daten — nur nach ausdrücklicher
  Aufforderung.
- Jedes Team meldet am Ende: Abnahme-Status Zeile für Zeile, offene Punkte, Übergabewerte.
