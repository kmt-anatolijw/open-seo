# M3 — Verbindung steht

**Ziel:** Hermes spricht mit der open-seo-Instanz: MCP eingetragen, Managed OAuth aktiv,
`whoami` liefert Credits.

|                |                                                                                                      |
| -------------- | ---------------------------------------------------------------------------------------------------- |
| Team           | [`kosmonaut-devops`](../skills/kosmonaut-devops/SKILL.md) + [DU], ausgeführt auf der Hermes-Maschine |
| Verfahren      | [Cloudflare-Runbook](../cloudflare-runbook.md), Schritt 5                                            |
| Setzt voraus   | M1 (Worker-Hostname), M2 (Hermes läuft)                                                              |
| Übergabe an M4 | funktionierender MCP-Zugang, dessen Refresh-Token headless trägt                                     |

## Auth-Weg (entschieden 18.08.2026 — Weg A: Managed OAuth)

D-09(d)-Befund der Hermes-Config-Session, verifiziert am Upstream-Code von
NousResearch/hermes-agent: `tools/mcp_oauth.py` beherrscht Discovery, Dynamic
Client Registration, PKCE und Token-Exchange (inkl. redirect_uri/Port-Cache
gegen den Restart-Fall); Tokens persistieren unter `HERMES_HOME/mcp-tokens/`
(liegt laut 04.3-Image-Layout im persistenten Volume, überlebt
Container-Recreates). Damit: **kein openseo-Patch, kein Service-Token, kein
Cloudflare-Dashboard-Moment.** Das email-Claim-Erfordernis des
Selfhost-MCP-Pfads ist erfüllt (Identity-JWT). Fallback bleibt Weg B
(Service-Token + openseo-Patch für common_name-Mapping), nur falls der
OAuth-Flow im Container praktisch hakt — dann Befund an den Orchestrator.

Hermes-seitiger M3-Scope zusätzlich: `openseo.tiaex.ai` in die
Egress-Allowlist des hermes-agent (erzwungener Egress-Proxy).

## Menschliche Schritte [DU]

- ~~Managed OAuth im Zero-Trust-Dashboard einschalten~~ **erledigt 16.08.2026**
  (Discovery auf `openseo.tiaex.ai/mcp` verifiziert)
- Einmalige interaktive MCP-Anmeldung auf der Hermes-Maschine (~1 Minute) —
  wird gebündelt erst abgerufen, wenn M2 durch ist und der Eintrag steht

## Arbeitspakete

1. `https://openseo.tiaex.ai/mcp` in `~/.hermes/config.yaml` eintragen. Das exakte
   Format des Eintrags aus ECCs `HERMES-SETUP.md` oder einer bestehenden `config.yaml`
   ablesen — nicht raten.
   **Gelöst (16.08.2026):** Super Bot Fight Mode auf `kosmonaut.io` blockiert
   headless Clients (empirisch: `/mcp`-POST → 403 `cf-mitigated: challenge`).
   Deshalb ist `openseo.tiaex.ai` als zweiter Worker-Hostname angebunden
   (`SELFHOST_DOMAIN` kommasepariert; Access-App deckt beide Hostnames) —
   Zone ohne SBFM, verifiziert: `/mcp`-POST liefert dort 401 mit
   `WWW-Authenticate: Bearer` + `resource_metadata` (Managed-OAuth-Discovery,
   aktiviert 16.08.2026). UI läuft auf `openseo.kosmonaut.io`, MCP auf
   `openseo.tiaex.ai` — Bot-Schutz der Firmen-Website unangetastet.
2. Die beiden [DU]-Schritte anfordern und begleiten.
3. `whoami` über den MCP aus Hermes heraus ausführen; Ergebnis an den Orchestrator.

## Abnahme

- [ ] `whoami` liefert Konto und verbleibende Credits — aus Hermes heraus, nicht vom
      Entwicklerrechner
- [ ] Zweiter `whoami`-Lauf in einer frischen Hermes-Sitzung, ohne Browser: das
      Refresh-Token trägt

## Bekannte Fehlerbilder

| Symptom                                         | Ursache                                                                  | Abhilfe                                   |
| ----------------------------------------------- | ------------------------------------------------------------------------ | ----------------------------------------- |
| Anmeldung klappt, aber **keine Tools** sichtbar | Managed OAuth aus — sieht aus wie ein kaputter Server, ist Konfiguration | Schalter in Zero Trust, Runbook Schritt 5 |
| Versuch mit `oseo_`-API-Key scheitert           | die Keys gelten nur für die gehostete Variante                           | OAuth-Weg nehmen, keine Keys              |

Teil des [Masterplans](./README.md).
