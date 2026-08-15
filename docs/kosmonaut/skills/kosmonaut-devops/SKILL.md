---
name: kosmonaut-devops
description: "Betrieb der open-seo-Instanz auf Cloudflare für den Kosmonaut SEO-Stack: deployen und aktualisieren über Alchemy, Cloudflare Access und Managed OAuth konfigurieren, MCP-Anbindung an Hermes, Secrets- und Zugriffsverwaltung, Fehlersuche über Worker-Logs, Abnahme vor dem ersten Kundenmandat. Triggert auf: Deployment, deployen, Cloudflare, Worker, Alchemy, Access, Zero Trust, MCP-Anbindung, Update, Rollout, Infrastruktur, DevOps, Zugriff, Secrets, Logs."
user-invocable: true
argument-hint: "[aufgabe]"
license: MIT
metadata:
  author: Kosmonaut
  version: "2.0.0"
  category: infrastructure
---

# Kosmonaut DevOps

Verantwortet die open-seo-Instanz, an der das SEO-Team hängt: Deployment auf Cloudflare,
Zugriff, Aktualisierung, Fehlersuche.

Vollständige Schrittfolge im [Cloudflare-Runbook](../../cloudflare-runbook.md).

## Feste Regeln

1. **Keine Secrets anfassen.** Schreibe `.env.selfhost` als Vorlage mit Platzhaltern und
   sage, welcher Wert wohin gehört. Trage nie einen echten Schlüssel ein, gib keinen in
   einer Antwort wieder, und lies keine `.env` aus, um ihren Inhalt zu zitieren.
2. **`alchemy destroy` nur nach ausdrücklicher Aufforderung.** Der Befehl löscht die
   stage-suffigierten D1-, KV- und R2-Ressourcen samt aller Daten.
3. **Zugriff wird in `.env.selfhost` gepflegt, nicht im Dashboard.** Änderungen an der
   Access-Policy (Allowlist) im UI werden beim nächsten Deploy überschrieben. Einzige
   gewollte Dashboard-Einstellung ist der Managed-OAuth-Schalter.
4. **Die IDs in `wrangler.jsonc` bleiben unverändert.** Sie gelten für lokale Entwicklung
   und Docker; miniflare leitet daraus per HMAC seine Dateinamen ab. Cloudflare-Deployments
   laufen ohnehin über `alchemy.run.ts` und lesen sie nicht.
5. **Vor jedem Deploy ansagen, was er bewirkt.** Nenne den Befehl und was passiert, wenn er
   fehlschlägt.

## Was hier läuft

open-seo als Cloudflare Worker, provisioniert über Alchemy. Ein `pnpm deploy:selfhost --yes`
legt D1, KV, R2, den Worker **und** die Cloudflare-Access-Application an, die genau die
Adressen aus `ACCESS_ALLOWED_EMAILS` durchlässt.

Die Anwendung bindet Workflows (`SiteAuditWorkflow`, `RankCheckWorkflow`), drei Durable
Objects, D1, zwei KV-Namespaces und R2 und importiert quer durch die Services aus
`cloudflare:workers`. Deshalb ist Cloudflare kein Hosting-Geschmack, sondern die Laufzeit,
für die die Anwendung geschrieben ist. **Vercel scheidet aus** — für Durable Objects und
Workflows gibt es dort kein Gegenstück.

### Warum nicht selbst hosten

Der Docker-Pfad ist technisch möglich (`Dockerfile.selfhost` fährt workerd im Container),
kostet aber zwei Plattformfunktionen:

- `AUTH_MODE=local_noauth` — kein Zugangsschutz, alles muss davor gebaut werden
- Die Cron-Trigger feuern nicht. Der Container serviert über `vite preview`, und der
  Vite-Cloudflare-Plugin validiert `triggers.crons` nur, ohne Scheduled-Events abzusetzen.
  `runScheduledRankChecks` und `reconcileStaleAudits` haben genau einen Aufrufer, den
  `scheduled`-Handler — Rank-Tracking liefe still ins Leere.

Beides fällt auf Cloudflare weg. Wer das Thema wieder aufmacht, sollte diese zwei Punkte
kennen, bevor er einen Server bestellt.

## Aufgabenbereiche

### Deployen und aktualisieren

`pnpm deploy:selfhost --yes` nach jedem Fork-Merge. Voraussetzung ist eine Anmeldung mit
`access:write`-Scope — ohne die kann der Deploy die Access-Application nicht anlegen, und
ein bloßes `pnpm alchemy login` fragt die Scopes nicht erneut ab. Dafür
`pnpm alchemy login --configure`.

Nach dem Deploy: Worker-URL öffnen, über Access anmelden, `/api/health` prüfen. Zusätzlich
prüfen, ob Managed OAuth noch aktiv ist — ob der Schalter ein Redeploy übersteht, ist
unverifiziert; das Fehlerbild wäre wieder „angemeldet, aber keine Tools".

### Zugriff verwalten

Teammitglied aufnehmen oder entfernen heißt: `ACCESS_ALLOWED_EMAILS` in `.env.selfhost`
ändern und neu deployen. Nicht im Zero-Trust-Dashboard editieren — das hält bis zum
nächsten Deploy.

Alle Zugelassenen teilen sich **einen** Workspace und sehen dieselben Projekte. Es gibt
keine Mandantentrennung innerhalb einer Instanz. Kommt die Anforderung auf, Kundendaten zu
trennen: mehrere Deployments, nicht Rollen innerhalb einer Instanz.

### MCP an Hermes anbinden

Managed OAuth ist für MCP-Clients erforderlich und standardmäßig aus. Fehlt es, meldet sich
der Client an und zeigt trotzdem **keine Tools** — das sieht nach einem defekten Server aus,
ist aber der Schalter in Zero Trust unter `Access controls` → Application →
`Additional settings` → `OAuth`. Dazu die Loopback-Redirect-URIs freigeben, weil CLI-Agents
`http://localhost:PORT/callback` registrieren.

Endpunkt ist `https://<worker-hostname>/mcp`. Die `oseo_`-API-Keys greifen hier nicht; sie
gelten nur für die gehostete Variante. Für die geplanten Läufe braucht es eine einmalige
interaktive Anmeldung, danach erneuert der Client über das Refresh-Token.

Läuft die Access-Sitzung ab, verlieren die Cron-Jobs **still** den Zugriff. Das ist der
Grund, warum der Credit-Wächter in `../../cron-jobs.md` einen Auth-Fehler ausdrücklich anders
meldet als einen niedrigen Guthabenstand.

### Fehler suchen

Worker-`Logs` im Dashboard oder `pnpm exec wrangler tail`. `/api/health` meldet
Laufzeitkonfiguration und Datenbankstatus und ist der erste Griff bei „die App tut nicht".

### Abnahme vor dem ersten Kundenmandat

- Anmeldung über Access funktioniert, eine nicht gelistete Adresse wird abgewiesen
- `/api/health` meldet die Datenbank als bereit
- Managed OAuth aktiv, Hermes sieht die open-seo-Tools, `whoami` liefert Credits
- Ein Rank-Tracker liefert innerhalb von zehn Minuten einen Messwert — der Beweis, dass die
  Plattform-Crons feuern
- `OPENSEO_TELEMETRY_DISABLED=1` gesetzt, falls so entschieden

Ist ein Punkt offen: nicht freigeben, sondern benennen, was fehlt.

## Wofür der Nutzer gebraucht wird

Klar benennen, wenn einer dieser Punkte ansteht, statt einen Umweg zu suchen:

- Cloudflare-Konto mit aktiviertem R2 — R2 verlangt eine hinterlegte Zahlungsmethode, auch
  im Free Tier
- `pnpm alchemy login` und `cloudflare bootstrap` — beide öffnen den Browser
- `DATAFORSEO_API_KEY` und `ACCESS_ALLOWED_EMAILS` in `.env.selfhost`
- Managed OAuth im Zero-Trust-Dashboard einschalten
- Einmalige interaktive MCP-Anmeldung auf der Hermes-Maschine

## Arbeitsweise

Erst den Ist-Zustand lesen — läuft der Worker, was sagt `/api/health`, ist Managed OAuth an
— dann handeln. Bei Abweichungen zwischen Runbook und Realität gilt die Realität; das
Runbook wird nachgezogen.
