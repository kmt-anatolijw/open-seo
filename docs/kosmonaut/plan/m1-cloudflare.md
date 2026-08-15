# M1 — open-seo läuft auf Cloudflare

**Ziel:** Eine erreichbare, zugriffsgeschützte open-seo-Instanz im eigenen
Cloudflare-Konto.

|                |                                                                                                       |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| Team           | [`kosmonaut-devops`](../skills/kosmonaut-devops/SKILL.md), auf der Maschine mit dem open-seo-Checkout |
| Verfahren      | [Cloudflare-Runbook](../cloudflare-runbook.md), Schritte 1–4                                          |
| Setzt voraus   | nichts — Startmeilenstein, parallel zu M2                                                             |
| Übergabe an M3 | Worker-Hostname; Bestätigung, dass Access aktiv ist                                                   |

## Menschliche Schritte [DU]

Vor dem Start bereitlegen (Details im Runbook, Schritt 1):

- Cloudflare-Konto mit **aktiviertem R2** — verlangt eine hinterlegte Zahlungsmethode,
  auch im Free Tier
- DataForSEO-Schlüssel

Während des Laufs:

- `pnpm alchemy login` (mit `access:write`!) und `cloudflare bootstrap` im Browser
  bestätigen
- `DATAFORSEO_API_KEY` und `ACCESS_ALLOWED_EMAILS` in `.env.selfhost` eintragen — das Team
  legt nur die Vorlage an

## Arbeitspakete

1. Voraussetzungen prüfen: Node ≥ 22.6, pnpm über corepack, Checkout aktuell
   (Fork-Merge gelaufen).
2. Runbook Schritte 2–3: Anmeldung mit `access:write`, Bootstrap, `.env.selfhost` als
   Vorlage anlegen. Die Werte trägt [DU] ein.
3. Runbook Schritt 4: `pnpm deploy:selfhost --yes` — vorher ansagen, was der Befehl
   anlegt.
4. Worker-Hostname und Deploy-Ausgabe an den Orchestrator melden.

## Abnahme

- [x] Anmeldung über Access funktioniert; eine nicht gelistete Adresse wird abgewiesen (anonym → 302 auf kmt-base.cloudflareaccess.com, geprüft 15.08.2026)
- [x] `https://open-seo-selfhost.kosmonaut-account.workers.dev/api/health` meldet `status: ok` (auth, dataforseo, database)
- [x] Worker-Hostname an den Orchestrator übergeben: `open-seo-selfhost.kosmonaut-account.workers.dev`

## Bekannte Fehlerbilder

| Symptom                                          | Ursache                       | Abhilfe                                                                                |
| ------------------------------------------------ | ----------------------------- | -------------------------------------------------------------------------------------- |
| Deploy kann die Access-Application nicht anlegen | Anmeldung ohne `access:write` | `pnpm alchemy login --configure` — ein bloßes Wiederholen fragt Scopes nicht erneut ab |
| Deploy scheitert am R2-Bucket                    | R2 im Konto nie aktiviert     | R2 einmal im Dashboard öffnen, Zahlungsmethode hinterlegen                             |

Teil des [Masterplans](./README.md).

## Ist-Stand der Umsetzung (15.08.2026) — Abweichungen vom Plan

- Auth lief über `alchemy login` (OAuth, Default-Scopes) statt API-Token; der
  Scope `access:write` ließ sich im Configure-Picker nicht zuverlässig setzen.
- Deshalb der im Deploy-Skript vorgesehene Alternativpfad: Access-Application
  `open-seo-selfhost.kosmonaut-account.workers.dev` (Policy `SelfHostAllow`,
  nur aw@kosmonaut.io) von Hand im Zero-Trust-Dashboard angelegt; `TEAM_DOMAIN`
  (mit https://-Präfix!) und `POLICY_AUD` in `.env.selfhost` → Alchemy
  provisioniert kein Access. Die E-Mail-Allowlist lebt jetzt in der
  Access-Policy, Änderungen dort pflegen (nicht `ACCESS_ALLOWED_EMAILS`).
- `pnpm-workspace.yaml`: sharp-Build bewusst ignoriert (nur transitive
  miniflare/wrangler-Abhängigkeit; pnpm 11 erzwingt sonst eine Entscheidung).
- Kein API-Token erzeugt — nichts zu löschen.
