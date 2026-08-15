# M3 — Verbindung steht

**Ziel:** Hermes spricht mit der open-seo-Instanz: MCP eingetragen, Managed OAuth aktiv,
`whoami` liefert Credits.

|                |                                                                                                      |
| -------------- | ---------------------------------------------------------------------------------------------------- |
| Team           | [`kosmonaut-devops`](../skills/kosmonaut-devops/SKILL.md) + [DU], ausgeführt auf der Hermes-Maschine |
| Verfahren      | [Cloudflare-Runbook](../cloudflare-runbook.md), Schritt 5                                            |
| Setzt voraus   | M1 (Worker-Hostname), M2 (Hermes läuft)                                                              |
| Übergabe an M4 | funktionierender MCP-Zugang, dessen Refresh-Token headless trägt                                     |

## Menschliche Schritte [DU]

- Managed OAuth im Zero-Trust-Dashboard einschalten und die Loopback-Redirect-URIs
  freigeben — Klickpfad im Runbook, Schritt 5
- Einmalige interaktive MCP-Anmeldung auf der Hermes-Maschine

## Arbeitspakete

1. `https://<worker-hostname>/mcp` in `~/.hermes/config.yaml` eintragen. Das exakte
   Format des Eintrags aus ECCs `HERMES-SETUP.md` oder einer bestehenden `config.yaml`
   ablesen — nicht raten.
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
