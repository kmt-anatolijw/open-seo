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

- [ ] Anmeldung über Access funktioniert; eine nicht gelistete Adresse wird abgewiesen
- [ ] `https://<worker-hostname>/api/health` meldet die Datenbank als bereit
- [ ] Worker-Hostname an den Orchestrator übergeben

## Bekannte Fehlerbilder

| Symptom                                          | Ursache                       | Abhilfe                                                                                |
| ------------------------------------------------ | ----------------------------- | -------------------------------------------------------------------------------------- |
| Deploy kann die Access-Application nicht anlegen | Anmeldung ohne `access:write` | `pnpm alchemy login --configure` — ein bloßes Wiederholen fragt Scopes nicht erneut ab |
| Deploy scheitert am R2-Bucket                    | R2 im Konto nie aktiviert     | R2 einmal im Dashboard öffnen, Zahlungsmethode hinterlegen                             |

Teil des [Masterplans](./README.md).
