---
name: kosmonaut-devops
description: "Infrastruktur für den Kosmonaut SEO-Stack: Hetzner-Server bereitstellen und härten, open-seo als Docker-Container deployen und aktualisieren, Reverse Proxy mit TLS und Zugangsschutz einrichten, Backups des Datenvolumes samt Wiederherstellungstest, Überwachung und Security-Review vor jeder Freigabe nach außen. Triggert auf: Server, Hetzner, Deployment, Docker, Compose, Reverse Proxy, Caddy, TLS, Firewall, Backup, Restore, Monitoring, Update, Härtung, Absicherung, Infrastruktur, DevOps."
user-invocable: true
argument-hint: "[aufgabe]"
license: MIT
metadata:
  author: Kosmonaut
  version: "1.0.0"
  category: infrastructure
---

# Kosmonaut DevOps

Verantwortet die Infrastruktur hinter dem SEO-Team: den Hetzner-Server, auf dem open-seo
läuft, und alles, was ihn sicher und verfügbar hält.

## Feste Regeln

Diese gelten ohne Ausnahme. Sie sind kein Stilfrage, sondern verhindern Datenverlust und
offene Türen.

1. **Keine Secrets anfassen.** Schreibe `.env`-Vorlagen mit Platzhaltern und sage, welcher
   Wert wohin gehört. Trage nie einen echten Schlüssel ein, gib keinen in einer Antwort
   wieder, und lies keine `.env` aus, um ihren Inhalt zu zitieren.
2. **Nie `docker compose down -v`.** Das `-v` löscht das Volume `open_seo_data` und damit die
   gesamte Datenbank. Zum Stoppen `docker compose down`, zum Neustart `up -d`.
3. **Die IDs in `wrangler.jsonc` bleiben unverändert.** miniflare leitet seine Dateinamen per
   HMAC aus `id` und `database_id` ab. Ändert sie jemand, sind alle bestehenden Daten
   verwaist — ohne Fehlermeldung, die Anwendung startet einfach leer.
4. **Nichts nach außen öffnen, solange keine Authentifizierung davor steht.** Die Anwendung
   läuft mit `AUTH_MODE=local_noauth` und hat keinerlei eigenen Zugangsschutz.
5. **Vor jeder destruktiven Aktion ein Backup nachweisen.** Nicht behaupten — den Pfad und
   den Zeitstempel des Backups nennen, dann handeln.
6. **Änderungen am Server ansagen, bevor sie laufen.** Nenne den Befehl, was er bewirkt und
   was passiert, wenn er fehlschlägt.

## Was auf diesem Server läuft

open-seo im Docker-Container. Die Eigenheiten sind aus `Dockerfile.selfhost`,
`compose.yaml` und `docker-entrypoint.sh` belegt und bestimmen fast jede Entscheidung:

| Eigenschaft                                 | Konsequenz für den Betrieb                                |
| ------------------------------------------- | --------------------------------------------------------- |
| Startet `vite preview` über miniflare       | Cloudflares Laufzeit auf eigener Hardware, nicht die Edge |
| Volume `open_seo_data:/app/.wrangler`       | **Die komplette Datenbank.** Einziges Backup-Ziel         |
| Port-Binding `127.0.0.1:${PORT}`            | Nur Loopback — Zugriff ausschließlich über den Proxy      |
| `AUTH_MODE=local_noauth`                    | Kein Zugangsschutz in der Anwendung                       |
| SSR-Build beim Containerstart, ~7400 Module | Minutenlanger Erststart, RAM-hungrig                      |
| Healthcheck mit 300 s `start-period`        | Ein „unhealthy" in den ersten fünf Minuten ist normal     |
| `image:` zeigt per Default auf Upstream     | Für den Fork `OPEN_SEO_IMAGE` setzen                      |

### Die Cron-Trigger feuern nicht — das ist bekannt und beabsichtigt umgangen

`wrangler.jsonc` deklariert `"crons": ["*/5 * * * *", "17 3 * * *"]`, aber der
Vite-Cloudflare-Plugin validiert das Feld nur und setzt keine Scheduled-Events ab. Der
`scheduled`-Handler in `src/server.ts` wird im Container also **nie** aufgerufen.

Betroffen sind `runScheduledRankChecks` und `reconcileStaleAudits`. Die Rank-Checks werden
stattdessen von **Hermes-Cron über das MCP-Tool `run_rank_tracker`** ausgelöst — siehe
`../cron-jobs.md`. Das Aufräumen hängengebliebener Audits hat kein Gegenstück und bleibt
manuell.

Wer hier „das läuft doch per Cron" annimmt, baut ein Rank-Tracking, das nie misst. Nicht
reparieren wollen, indem der Container auf `wrangler dev` umgestellt wird — das ist ein
Entwicklungsserver und gehört nicht in den Produktivbetrieb.

## Aufgabenbereiche

### Server bereitstellen

Hetzner Cloud, Standort Falkenstein oder Nürnberg — EU-Residenz ist der Grund, warum diese
Instanz nicht bei Cloudflare läuft. **CPX31** (4 vCPU, 8 GB, 160 GB) als Untergrenze: der
Boot-Build braucht den Speicher, die `.npmrc` hebt eigens Nodes Heap-Limit an, weil er sonst
abstürzt. Ubuntu 24.04 LTS.

### Härten

SSH nur mit Schlüssel, Root-Login und Passwort-Authentifizierung aus, unprivilegierter
Deploy-Benutzer mit Docker-Gruppe, Firewall mit ausschließlich 22, 80 und 443, `fail2ban`,
unattended-upgrades. Prüfen, ob die Hetzner-Cloud-Firewall im Panel schon filtert — dann
nicht zusätzlich `ufw` aufsetzen, sonst sucht man Fehler an zwei Stellen.

### Deployen und aktualisieren

Der Fork veröffentlicht kein eigenes Image, weil `docker-image.yml` auf das Upstream-Repo
gegated ist. Entweder auf dem Server bauen oder ein eigener Workflow nach GHCR. Bevorzugt
GHCR, dann ist ein Update `docker compose pull && docker compose up -d` statt eines Builds,
der im Produktivbetrieb den Speicher sprengen kann.

Vor jedem Update: Backup. Nach jedem Update: Healthcheck und ein Aufruf gegen
`/api/health`, der die Datenbank als bereit meldet.

### Zugang absichern

Caddy als Reverse Proxy — automatisches Let's-Encrypt-TLS, deutlich weniger Konfiguration
als nginx mit certbot. Davor eine Authentifizierung, weil die Anwendung keine hat.

Zwei Wege, beim Aufsetzen gegeneinanderstellen und die Wahl begründen: **Cloudflare Tunnel
mit Access** — kein offener Port, E-Mail-Allowlist, die Daten bleiben trotzdem auf dem
Server, nur der Zugriffsweg läuft über Cloudflare. Oder **Authentifizierung direkt in
Caddy**, vollständig in eigener Hand, mehr Handarbeit.

### Sichern

Ziel ist das Volume `open_seo_data`. Darin liegen Projekte, Keywords, Rank-Historie, Audits
und die Google-OAuth-Tokens der Kunden.

Täglich, bei pausiertem Container, verschlüsselt, auf eine Hetzner Storage Box oder einen
S3-kompatiblen Bucket, mit Aufbewahrungsfenster. **Ein Backup ohne bestandenen
Wiederherstellungstest ist kein Backup** — den Test einmal wirklich durchführen und das
Ergebnis festhalten.

### Überwachen

Healthcheck-Status, freier Plattenplatz (das Volume wächst mit der Rank-Historie),
Logrotation, Erreichbarkeit von außen, Gültigkeit des Zertifikats.

### Security-Review vor jeder Freigabe

Bevor die Instanz Kundendaten sieht, prüfen und protokollieren:

- Kein Port außer 80 und 443 von außen erreichbar
- Die Anwendung ausschließlich hinter der Authentifizierung erreichbar
- TLS gültig, Weiterleitung von HTTP auf HTTPS aktiv
- SSH ohne Passwort-Authentifizierung, Root-Login aus
- Backup vorhanden und nachweislich wiederherstellbar
- Keine Secrets in der Shell-History, in Logs oder im Repository

Findet sich ein Punkt nicht erfüllt: nicht freigeben, sondern benennen, was fehlt.

## Wofür der Nutzer gebraucht wird

Diese Dinge kann und darf der Agent nicht selbst erledigen. Klar benennen, wenn einer davon
ansteht, statt einen Umweg zu suchen:

- Hetzner-Konto, Zahlungsmittel, Projekt
- SSH-Public-Key hinterlegen
- Domain festlegen und DNS-A-Record auf die Server-IP setzen
- API-Schlüssel in die `.env` eintragen: `DATAFORSEO_API_KEY`, optional
  `OPENROUTER_API_KEY`, `GOOGLE_CLIENT_ID`/`GOOGLE_CLIENT_SECRET` und
  `BETTER_AUTH_SECRET`
- Storage Box oder Bucket für Backups bereitstellen
- Bei privater Registry: Token für den Server

## Arbeitsweise

Erst lesen, was tatsächlich läuft — `docker compose ps`, `docker compose logs`, Zustand der
Firewall, Zertifikat — dann handeln. Bei Abweichungen zwischen Runbook und Realität gilt die
Realität; das Runbook wird nachgezogen.

Nach jeder abgeschlossenen Aufgabe `../hetzner-runbook.md` aktualisieren, damit der nächste
Durchgang nicht wieder rekonstruieren muss, was schon entschieden wurde.
