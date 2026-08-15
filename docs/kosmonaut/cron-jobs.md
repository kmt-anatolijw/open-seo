# Geplante Läufe — Spezifikation

Was Hermes-Cron ausführen soll, wann, mit welchem Prompt und wer das Ergebnis bekommt.
Gehört zu [`seo-team-hermes.md`](./seo-team-hermes.md).

> **Warum hier kein fertiges `jobs.json` steht.** Das Schema von `~/.hermes/cron/jobs.json`
> ist öffentlich nicht dokumentiert. ECCs Migrationsguide behandelt Hermes-Cron nur als
> Importquelle (`ecc migrate import-schedules --source ~/.hermes --dry-run`) und zeigt kein
> Beispiel. Ein von mir erfundenes Schema wäre geraten und würde beim ersten Lauf brechen.
>
> Den echten Aufbau bekommt ihr aus einer bestehenden `jobs.json` oder aus
> `ecc migrate audit --source ~/.hermes`. Die Prompts unten lassen sich dann eintragen —
> das ist der Teil, der Denkarbeit kostet.

## Übersicht

| Job            | Rhythmus             | Runtime | Meldung                 |
| -------------- | -------------------- | ------- | ----------------------- |
| Drift-Wache    | täglich, 06:15       | Hermes  | nur bei Abweichung      |
| Monatsaudit    | 1. des Monats, 04:00 | Hermes  | immer, als PDF          |
| Credit-Wächter | montags, 08:00       | Hermes  | nur bei Unterschreitung |

Zeiten bewusst vor Arbeitsbeginn und versetzt, damit sich parallele Läufe nicht um
Chromium-Instanzen und API-Kontingente streiten.

> **Rank-Checks stehen bewusst nicht in dieser Liste.** open-seo erledigt sie selbst: die
> Cron-Trigger aus `wrangler.jsonc` — alle fünf Minuten Rank-Checks und
> Stale-Audit-Reconcile — feuern auf Cloudflare als Plattformfunktion. Sie hier
> nachzubauen würde die Arbeit verdoppeln und Credits verbrennen.
>
> Das gilt **nur** für das Cloudflare-Deployment. Wer die Instanz je selbst hostet, verliert
> diese Trigger: der Docker-Container serviert über `vite preview`, und der
> Vite-Cloudflare-Plugin validiert `triggers.crons` lediglich, ohne Scheduled-Events
> abzusetzen. Dann braucht es hier einen zusätzlichen Job, der je Projekt
> `run_rank_tracker` über MCP aufruft.

---

## Job 1 — Drift-Wache

**Zweck:** Unbemerkte Veränderungen an Kundenseiten finden, bevor der Kunde sie findet.
Das ist der Job mit dem besten Verhältnis von Aufwand zu Nutzen — er läuft auf claude-seos
gespeicherter Baseline und kostet keine DataForSEO-Credits.

**Rhythmus:** täglich, 06:15
**Runtime:** Hermes
**Voraussetzung:** je Domain einmalig `/seo drift baseline <domain>` gelaufen

**Prompt:**

> Führe `/seo drift compare` für jede Domain in der Kundenliste aus. Melde ausschließlich
> Domains mit Abweichungen der Schwere hoch oder mittel. Fasse je betroffener Domain in
> höchstens drei Sätzen zusammen: was sich geändert hat, seit wann, und ob es Handlung
> erfordert. Gibt es nirgends eine Abweichung dieser Schwere, antworte mit genau
> `KEINE ABWEICHUNG` und sonst nichts.

**Empfänger:** OpenClaw, aber nur wenn die Antwort **nicht** `KEINE ABWEICHUNG` ist.

**Abbruchbedingung:** Sind mehr als die Hälfte der Domains nicht erreichbar, den Lauf
abbrechen und das als Infrastrukturproblem melden statt als SEO-Drift — sonst erzeugt ein
Netzwerkausfall einen Fehlalarm über alle Mandate hinweg.

**Stille ist Absicht.** Ein täglicher Job, der jeden Tag meldet, wird nach zwei Wochen
ignoriert. Er meldet sich nur, wenn etwas ist.

---

## Job 2 — Monatsaudit

**Zweck:** Der vollständige Durchlauf je Mandat, der als Beleg gegenüber dem Kunden dient.

**Rhythmus:** 1. des Monats, 04:00
**Runtime:** Hermes, delegiert selbst parallel an bis zu 15 Subagents

**Prompt:**

> Führe `/seo audit` für jede Domain in der Kundenliste aus, eine Domain nach der anderen.
> Erzeuge je Domain einen PDF-Bericht über `/seo google report` und prüfe vor der Ablage,
> dass die eingebaute Review `"status": "PASS"` meldet. Rufe open-seo nur, wenn das Audit
> eine Frage aufwirft, die Marktdaten oder Historie braucht — nenne in dem Fall vorher die
> zu erwartenden Credits. Lege die Berichte unter `~/.hermes/workspace/seo/<domain>/<jahr-monat>/`
> ab und schreibe je Domain drei Sätze in den Memory Vault: Gesamtbild, größte Veränderung
> gegenüber dem Vormonat, empfohlener nächster Schritt.

**Empfänger:** OpenClaw mit Sammelmeldung und Ablagepfad, Details im PDF.

**Abbruchbedingung:** Schlägt die PDF-Review fehl, das PDF **nicht** ablegen und die Domain
in der Sammelmeldung als offen kennzeichnen. Ein Bericht mit leeren Diagrammen ist schlimmer
als kein Bericht.

**Reihenfolge, nicht parallel.** Jedes `/seo audit` startet bereits bis zu 15 Subagents.
Mehrere Domains gleichzeitig würden Hermes' Concurrency-Grenze sprengen und die Läufe
gegenseitig ausbremsen.

---

## Job 3 — Credit-Wächter

**Zweck:** Verhindern, dass ein Monatsaudit oder eine Recherche mitten im Lauf an
erschöpften DataForSEO-Credits scheitert.

**Rhythmus:** montags, 08:00
**Runtime:** Hermes

**Prompt:**

> Rufe `whoami` gegen den open-seo-MCP auf. Liegt das verbleibende Guthaben unter der
> Schwelle für einen vollständigen Monatslauf, melde den Stand und die Reichweite in Tagen.
> Liegt es darüber, antworte mit genau `CREDITS OK` und sonst nichts.

**Empfänger:** OpenClaw, nur wenn die Antwort nicht `CREDITS OK` ist.

**Abbruchbedingung:** Antwortet der MCP nicht oder scheitert die Authentifizierung, als
**Verbindungs- bzw. Auth-Problem** melden — nicht als Guthabenproblem. Ein abgelaufener
Access-Zugang sieht sonst aus wie ein voller Credit-Stand und legt unbemerkt auch Job 0 lahm.

---

## Vor dem Scharfstellen

1. Je Kundendomain einmalig `/seo drift baseline <domain>` — ohne Baseline hat Job 1 nichts
   zu vergleichen und meldet jeden Tag einen Fehler.
2. Die Kundenliste an einer Stelle pflegen, auf die alle drei Prompts verweisen, statt
   Domains in drei Jobs zu duplizieren.
3. Zugang zum open-seo-MCP von der Hermes-Maschine aus prüfen. Die `oseo_`-API-Keys greifen
   hier **nicht** — sie hängen am Hosted-Modus. Auf der self-hosted Instanz führt der Weg
   über den Reverse Proxy und dessen Authentifizierung; bei Cloudflare Access ist das eine
   einmalige interaktive Anmeldung, danach erneuert der Client über das Refresh-Token.
   Läuft die Access-Sitzung ab, verlieren **alle** Jobs still den Zugriff — deshalb meldet
   Job 3 einen Auth-Fehler ausdrücklich anders als einen niedrigen Guthabenstand.
4. Jeden Job einmal von Hand auslösen, bevor der Zeitplan greift. Ein Cronjob, der zum
   ersten Mal um 04:00 fehlschlägt, wird erst am nächsten Werktag bemerkt.
