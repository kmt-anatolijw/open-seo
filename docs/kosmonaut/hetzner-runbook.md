# Hetzner-Runbook: open-seo self-hosted

Schrittfolge vom leeren Konto bis zur freigegebenen Instanz. Ausführung durch den
[`kosmonaut-devops`](./skills/kosmonaut-devops/SKILL.md)-Agenten.

**[DU]** markiert Schritte, die nur ihr erledigen könnt — Konto, Zahlungsmittel, DNS,
Zugangsdaten. Alles ohne Markierung übernimmt der Agent.

Gehört zu [`seo-team-hermes.md`](./seo-team-hermes.md).

---

## Warum Hetzner und nicht Cloudflare

`Dockerfile.selfhost` fährt **workerd** im Container — Cloudflares Workers-Laufzeit auf
eigener Hardware. Deshalb ist EU-Residenz überhaupt erreichbar. Vercel schied aus: die
Anwendung bindet Workflows, Durable Objects und importiert quer durch die Services aus
`cloudflare:workers`.

Der Preis steht in Schritt 0 und Schritt 5: keine funktionierenden Cron-Trigger, keine
eigene Authentifizierung.

---

## Schritt 0 — Erledigt: die Cron-Trigger feuern nicht

Geprüft, nicht vermutet. Vier Belege:

1. `docker-entrypoint.sh` endet mit `exec pnpm exec vite preview` — miniflare, nicht die Edge.
2. Der Laufzeitcode von `@cloudflare/vite-plugin@1.42.3` enthält weder `__scheduled` noch
   `cdn-cgi/handler/scheduled` noch einen Scheduled-Dispatch. Die einzigen `crons`-Treffer
   sind Config-Validierung (`validateTriggers`).
3. `runScheduledRankChecks` und `reconcileStaleAudits` haben genau einen Aufrufer: den
   `scheduled`-Handler in `src/server.ts:223/229`.
4. Der einzige Durable-Object-Alarm ist die Scratchpad-Aufräumung in
   `AuditScratchpad.ts:326` — nicht die Rank-Checks.

**Folge und Umgehung:** Rank-Checks werden von **Hermes-Cron über das MCP-Tool
`run_rank_tracker`** ausgelöst, das intern dieselbe `RankTrackingService.triggerCheck()`
aufruft wie der Cron. Kein Eingriff in den Fork nötig. Details in
[`cron-jobs.md`](./cron-jobs.md).

**Offener Restpunkt:** `reconcileStaleAudits` hat kein MCP-Gegenstück. Audits, deren
Workflow stirbt, bleiben auf „running" stehen. Kein Datenverlust, aber gelegentlich manuell
aufzuräumen.

---

## Schritt 1 — Server bestellen

**[DU]** Hetzner-Cloud-Konto anlegen, Zahlungsmittel hinterlegen, Projekt erstellen.
**[DU]** SSH-Public-Key im Projekt hinterlegen.

| Einstellung | Wert                             | Begründung                                                                                           |
| ----------- | -------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Typ         | **CPX31** — 4 vCPU, 8 GB, 160 GB | Der SSR-Build umfasst ~7400 Module; die `.npmrc` hebt eigens Nodes Heap-Limit an. 4 GB sind zu knapp |
| Standort    | Falkenstein oder Nürnberg        | EU-Residenz ist der Grund für diesen Weg                                                             |
| Image       | Ubuntu 24.04 LTS                 |                                                                                                      |
| Netzwerk    | IPv4 + IPv6                      |                                                                                                      |

**[DU]** Entscheiden, ob die Hetzner-Cloud-Firewall im Panel filtert oder `ufw` auf dem
Server. Nicht beides — sonst sucht man Fehler an zwei Stellen.

## Schritt 2 — Härten

Der Agent richtet ein:

- Deploy-Benutzer ohne Root-Rechte, in der Docker-Gruppe
- SSH: nur Schlüssel, `PermitRootLogin no`, `PasswordAuthentication no`
- Firewall: eingehend ausschließlich 22, 80, 443
- `fail2ban` für SSH
- `unattended-upgrades` für Sicherheitsaktualisierungen
- Docker Engine und Compose-Plugin

Abnahme: Anmeldung als Deploy-Benutzer mit Schlüssel funktioniert, Anmeldung mit Passwort
wird abgewiesen, `docker run hello-world` läuft ohne `sudo`.

## Schritt 3 — Image bereitstellen

Der Fork veröffentlicht kein Image, weil `docker-image.yml` auf `every-app/open-seo` gegated
ist. Empfohlen: eigener Workflow nach GHCR, damit ein Update später ein `pull` ist und kein
Build auf der Produktionsmaschine.

- Neuer Workflow im Fork, ohne das Upstream-Gate, Ziel `ghcr.io/kmt-anatolijw/open-seo`
- Auf dem Server `OPEN_SEO_IMAGE` auf dieses Image setzen

**[DU]** Falls das Paket privat sein soll: Registry-Token erzeugen und auf dem Server
hinterlegen.

Alternative ohne Registry: Fork auf den Server klonen und `docker compose build`. Spart die
Pipeline, verlagert aber die Build-Last samt Speicherbedarf in den Produktivbetrieb.

## Schritt 4 — Deployen

Der Agent legt `.env` als Vorlage mit Platzhaltern an. **[DU]** trägt die Werte ein:

| Variable                                                           | Pflicht   | Zweck                                              |
| ------------------------------------------------------------------ | --------- | -------------------------------------------------- |
| `DATAFORSEO_API_KEY`                                               | ja        | base64-kodierte Zugangsdaten                       |
| `ALLOWED_HOST`                                                     | ja        | eure Domain, sonst weist der Preview-Server sie ab |
| `OPENSEO_TELEMETRY_DISABLED`                                       | empfohlen | `1` schaltet den anonymen Nutzungs-Heartbeat ab    |
| `OPENROUTER_API_KEY`                                               | optional  | AI-Funktionen (SAM, Onboarding-Chat)               |
| `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` / `BETTER_AUTH_SECRET` | optional  | Search-Console-Anbindung                           |

Dann `docker compose up -d`.

**Der Erststart dauert Minuten**: Preflight, Migrationen, danach der vollständige Build. Der
Healthcheck hat deshalb 300 Sekunden `start-period`. Ein „unhealthy" in den ersten fünf
Minuten ist kein Fehler. Fortschritt mit `docker compose logs -f` verfolgen.

Abnahme: `curl http://127.0.0.1:3001/api/health` meldet die Datenbank als bereit.

## Schritt 5 — Reverse Proxy, TLS, Zugangsschutz

**Der sicherheitskritische Schritt.** Die Anwendung läuft mit `AUTH_MODE=local_noauth` und
hat keinerlei eigenen Zugangsschutz. Wer den Port erreicht, hat vollen Zugriff auf alle
Kundenprojekte und kann das DataForSEO-Guthaben verbrauchen.

**[DU]** Domain oder Subdomain festlegen, A-Record auf die Server-IP setzen.

Der Agent richtet Caddy ein — automatisches Let's-Encrypt-TLS, deutlich weniger
Konfiguration als nginx mit certbot — und davor eine Authentifizierung. Zwei Wege, beim
Aufsetzen mit Begründung wählen:

| Weg                                             | Vorteil                                                                                                       | Preis                                      |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| **Cloudflare Tunnel + Access**                  | kein offener Port nach außen, E-Mail-Allowlist wie bei der Workers-Variante; die Daten bleiben auf dem Server | der Zugriffsweg läuft über Cloudflare      |
| **Auth direkt in Caddy** (Basic Auth oder OIDC) | vollständig in eigener Hand                                                                                   | mehr Handarbeit, Benutzerverwaltung selbst |

Das Port-Binding bleibt auf `127.0.0.1`, wie es die `compose.yaml` bereits vorgibt.

Abnahme: Zugriff auf die Domain landet an der Authentifizierung; ein direkter Zugriff auf
den Port von außen scheitert.

## Schritt 6 — Rank-Checks über Hermes anstoßen

Folgt aus Schritt 0. Kein System-Cron auf dem Server, sondern ein Hermes-Job, der je Projekt
`run_rank_tracker` über MCP aufruft. Rhythmus und Prompt in [`cron-jobs.md`](./cron-jobs.md).

Der Vorteil dieser Lösung: der Zeitplan liegt dort, wo auch die übrigen SEO-Jobs liegen, und
der Fork bleibt unverändert.

## Schritt 7 — Backups

Ziel ist ausschließlich das Volume `open_seo_data` (`/app/.wrangler`). Darin liegen
Projekte, Keywords, Rank-Historie, Audits und die Google-OAuth-Tokens eurer Kunden.

**[DU]** Hetzner Storage Box oder S3-kompatiblen Bucket bereitstellen, Zugangsdaten setzen.

Der Agent richtet ein: täglicher Lauf bei pausiertem Container, verschlüsselt, mit
Aufbewahrungsfenster — **und einen tatsächlich durchgeführten Wiederherstellungstest**. Ohne
den ist es kein Backup, sondern eine Datei.

> Niemals `docker compose down -v`. Das `-v` löscht genau dieses Volume.

## Schritt 8 — Betrieb

| Aufgabe  | Vorgehen                                                           |
| -------- | ------------------------------------------------------------------ |
| Update   | Backup, dann `docker compose pull && docker compose up -d`         |
| Logs     | `docker compose logs -f --tail=200`                                |
| Neustart | `docker compose restart` (Build wird per Fingerprint übersprungen) |
| Zustand  | `docker compose ps`, `/api/health`                                 |

Upstream-Änderungen kommen weiterhin per Sync-PR im Fork an. Das Deployment bleibt eine
bewusste Entscheidung, kein Automatismus.

Im Blick behalten: Plattenplatz — das Volume wächst mit der Rank-Historie.

## Schritt 9 — Security-Review vor der Freigabe

Erst nach vollständig abgehaktem Review sieht die Instanz Kundendaten.

- [ ] Von außen ausschließlich 80 und 443 erreichbar
- [ ] Anwendung nur hinter der Authentifizierung erreichbar
- [ ] TLS gültig, HTTP leitet auf HTTPS um
- [ ] SSH ohne Passwort, Root-Login aus
- [ ] Backup vorhanden **und** Wiederherstellung erfolgreich getestet
- [ ] Keine Secrets in Shell-History, Logs oder Repository
- [ ] `OPENSEO_TELEMETRY_DISABLED=1` gesetzt, falls so entschieden

Ist ein Punkt offen: nicht freigeben, sondern benennen, was fehlt.
