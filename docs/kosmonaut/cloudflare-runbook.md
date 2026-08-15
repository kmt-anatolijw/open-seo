# Cloudflare-Runbook: open-seo deployen

Vom leeren Cloudflare-Konto bis zur laufenden Instanz, an der Hermes hängt.
Ausführung durch den [`kosmonaut-devops`](./skills/kosmonaut-devops/SKILL.md)-Agenten.

**[DU]** markiert Schritte, die nur ihr erledigen könnt — Konto, Zahlungsmittel, Anmeldung
im Browser, Schlüssel. Alles ohne Markierung übernimmt der Agent.

Gehört zu [`seo-team-hermes.md`](./seo-team-hermes.md).

---

## Warum Cloudflare

Es ist der Weg, für den open-seo gebaut ist. Die Anwendung bindet Cloudflare Workflows
(`SiteAuditWorkflow`, `RankCheckWorkflow`), drei Durable Objects, D1, zwei KV-Namespaces und
R2, und importiert quer durch die Services aus `cloudflare:workers`.

**Vercel scheidet damit aus.** Für Durable Objects und Workflows gibt es dort kein
Gegenstück; ein Wechsel wäre das Neuschreiben der Persistenz- und Hintergrundjob-Schicht,
und jeder Upstream-Sync würde danach kollidieren.

Zwei Dinge, die auf Cloudflare mitkommen und im Self-Hosting fehlen würden:

- **Zugangsschutz.** Der Deploy legt die Cloudflare-Access-Application gleich mit an.
  Der Docker-Pfad läuft dagegen mit `AUTH_MODE=local_noauth`, ganz ohne Schutz.
- **Funktionierende Cron-Trigger.** Auf der Edge feuern die in `wrangler.jsonc`
  deklarierten Zeitpläne — alle 5 Minuten Rank-Checks und Stale-Audit-Reconcile, täglich
  die OAuth-Aufräumung. Im Docker-Container feuern sie nicht: der serviert über
  `vite preview`, und der Vite-Cloudflare-Plugin validiert `triggers.crons` lediglich,
  ohne Scheduled-Events abzusetzen. Rank-Tracking liefe dort still ins Leere.

Läuft laut Projektdoku auf dem **Free Plan**.

---

## Schritt 1 — Voraussetzungen

**[DU]** Cloudflare-Konto mit **aktiviertem R2**. R2 verlangt eine hinterlegte
Zahlungsmethode, auch im Free Tier — wer es nie benutzt hat, muss es einmal im Dashboard
öffnen.

**[DU]** DataForSEO-Konto anlegen, Schlüssel bereithalten (`docs/DATAFORSEO_API_KEY.md`).

Lokal: Node 22.6 oder neuer, pnpm über `corepack enable`.

## Schritt 2 — Einmalige Anmeldung

```bash
pnpm alchemy login                # „Customize OAuth scopes?" mit ja beantworten,
                                  # access:write aktivieren
pnpm alchemy cloudflare bootstrap # State-Store-Worker ins Konto deployen
```

**[DU]** Beides öffnet den Browser und braucht eure Zustimmung.

Wer schon ohne `access:write` angemeldet ist: `pnpm alchemy login --configure`. Ein
schlichtes Wiederholen fragt die Scopes **nicht** erneut ab — der häufigste Stolperstein,
weil der Deploy sonst die Access-Application nicht anlegen kann.

## Schritt 3 — Konfiguration

```bash
cp .env.selfhost.example .env.selfhost
```

Die Vorlage hat genau zwei Felder. **[DU]** trägt beide ein:

| Variable                | Zweck                                                |
| ----------------------- | ---------------------------------------------------- |
| `DATAFORSEO_API_KEY`    | base64-kodierte Zugangsdaten                         |
| `ACCESS_ALLOWED_EMAILS` | Allowlist — genau diese Adressen kommen durch Access |

Optional, für den Agenturbetrieb erwägenswert: `OPENSEO_TELEMETRY_DISABLED=1`. Die
Telemetrie sendet laut Doku keine URLs, Keywords oder E-Mails, aber Zählwerte an eine
Install-ID.

**Bewährter Alternativpfad (so lief M1):** Fehlt dem OAuth-Login der Scope
`access:write` (der Configure-Picker setzt ihn leicht daneben), Access-Application
von Hand im Zero-Trust-Dashboard anlegen — Self-hosted-App auf
`open-seo-<stage>.<subdomain>.workers.dev`, Allow-Policy mit den erlaubten
E-Mails — und zwei weitere Variablen setzen:

| Variable      | Zweck                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------ |
| `TEAM_DOMAIN` | Zero-Trust-Team, **volle URL mit https://**, z. B. `https://kmt-base.cloudflareaccess.com` |
| `POLICY_AUD`  | AUD-Tag der App (Zero Trust → Applications → App → Additional settings)                    |

Sind beide gesetzt, provisioniert der Deploy kein Access — die E-Mail-Allowlist
lebt dann in der Access-Policy im Dashboard, `ACCESS_ALLOWED_EMAILS` ist wirkungslos.
Häufigster Fehler: `TEAM_DOMAIN` ohne `https://` → `/api/health` meldet einen
Auth-Konfigurationsfehler statt `ok`.

## Schritt 4 — Deployen

```bash
pnpm deploy:selfhost --yes
```

Ein Befehl provisioniert die D1-Datenbank, die KV-Namespaces und den R2-Bucket, spielt die
Migrationen ein, deployt den Worker und legt die **Cloudflare-Access-Application** an, die
genau `ACCESS_ALLOWED_EMAILS` durchlässt. Gibt es im Konto noch kein Zero-Trust-Team, wird
eines angelegt, benannt nach der `workers.dev`-Subdomain.

Wer die Access-Application lieber selbst verwaltet, setzt `TEAM_DOMAIN` und `POLICY_AUD` in
`.env.selfhost` — dann provisioniert der Deploy keine Access-Ressourcen.

**Abnahme:** Worker-URL aus der Ausgabe öffnen, über Access anmelden, open-seo lädt.
`https://<worker-hostname>/api/health` meldet Laufzeitkonfiguration und Datenbankstatus.

## Schritt 5 — MCP freischalten

**Der Schritt, den man übersieht.** Managed OAuth ist für MCP-Clients erforderlich und
standardmäßig aus. Ohne ihn meldet sich Hermes erfolgreich an und zeigt anschließend
**keine Tools** — ein Fehlerbild, das nach einem kaputten Server aussieht, aber
Konfiguration ist.

**[DU]** im Cloudflare-Dashboard: Zero Trust → `Access controls` → `Applications` → die
OpenSEO-Application → `Edit` → `Additional settings` → `OAuth` → `Managed OAuth`
einschalten. Dann unter `Managed OAuth settings` die Redirect-URIs freigeben:

- `localhost` bzw. Loopback für CLI- und Desktop-Agents — Hermes, Claude Code und Codex CLI
  registrieren `http://localhost:PORT/callback`; den Port wählt der Client je Sitzung,
  Loopback deshalb generell freigeben
- HTTPS-Redirect-URIs nur für Web-Connectors (für reinen CLI-Betrieb reicht Loopback);
  ein Pfad darf auf `/*` enden

Endpunkt für Hermes, einzutragen in `~/.hermes/config.yaml` (andere MCP-Clients nutzen
ihre eigene Registry, etwa `mcp-configs/mcp-servers.json`):

```text
https://<worker-hostname>/mcp
```

**[DU]** Einmalige interaktive Anmeldung des MCP-Clients auf der Hermes-Maschine. Danach
erneuert der Client über das Refresh-Token ohne Browser — das ist die Grundlage dafür, dass
die geplanten Läufe aus [`cron-jobs.md`](./cron-jobs.md) headless funktionieren.

Die `oseo_`-API-Keys helfen hier nicht: sie hängen an `oauth-provider.ts` mit
`getHostedBaseUrl()` und gelten nur für die gehostete Variante unter `app.openseo.so`.

**Abnahme:** `whoami` über den MCP liefert Konto und verbleibende Credits. Ein zweiter
Lauf ohne Browser bestätigt, dass das Refresh-Token trägt.

## Schritt 6 — Betrieb

| Aufgabe                | Vorgehen                                                         |
| ---------------------- | ---------------------------------------------------------------- |
| Aktualisieren          | Fork-Merge, `pnpm install`, `pnpm deploy:selfhost --yes`         |
| Teammitglied aufnehmen | `ACCESS_ALLOWED_EMAILS` ergänzen, neu deployen                   |
| Fehler suchen          | Worker-`Logs` im Dashboard oder `pnpm exec wrangler tail`        |
| Abbauen                | `pnpm alchemy destroy --env-file .env.selfhost --stage selfhost` |

Drei Dinge, die im Agenturbetrieb zählen:

- **Dashboard-Änderungen an der Access-Policy werden beim nächsten Deploy überschrieben.**
  Zugriff wird in `.env.selfhost` gepflegt, nicht im UI. Ausnahme ist der
  Managed-OAuth-Schalter aus Schritt 5: er lebt im Dashboard — nach jedem Deploy prüfen,
  ob er noch aktiv ist; ob er ein Redeploy übersteht, ist unverifiziert (Fehlerbild:
  angemeldet, aber keine Tools).
- **Alle Zugelassenen arbeiten in einem gemeinsamen Workspace** und sehen dieselben
  Projekte. Innerhalb einer Instanz gibt es keine Mandantentrennung; wer Kundendaten
  trennen muss, deployt mehrfach.
- `alchemy destroy` löscht die stage-suffigierten D1-, KV- und R2-Ressourcen **samt Daten**.

## Schritt 7 — Abnahme vor dem ersten Mandat

- [ ] Anmeldung über Access funktioniert, eine nicht gelistete Adresse wird abgewiesen
- [ ] `/api/health` meldet die Datenbank als bereit
- [ ] Managed OAuth aktiv, Hermes sieht die open-seo-Tools
- [ ] `whoami` liefert Credits
- [ ] Ein Rank-Tracker angelegt und nach spätestens 10 Minuten mit einem Messwert versehen —
      das ist der Beweis, dass die Plattform-Crons feuern
- [ ] `OPENSEO_TELEMETRY_DISABLED=1` gesetzt, falls so entschieden
