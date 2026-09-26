# PortalCRM

Entwicklungs-Repository für das Portal (Mieter, Eigentümer inkl. Beirat, Dienstleister) der
MH Verwaltungsplattform (`mhvp`). Das Hauptprojekt ist
[v3ni94/CRM-HV-Verwaltungssoftware](https://github.com/v3ni94/CRM-HV-Verwaltungssoftware);
dessen API (FastAPI, `apps/api`) versorgt das Portal mit allen Daten.

## Rollenverteilung der Repositories

| Repository | Rolle |
| --- | --- |
| CRM-HV-Verwaltungssoftware | Hauptprojekt: API, web-crm, Pakete, Infrastruktur, Deployment |
| PortalCRM (dieses Repo) | Entwicklung und Verbesserung des Portals (`apps/web-portal`) |

Sync-Richtung, verbindlich:

- `apps/web-portal/` wird **hier** entwickelt und automatisch als Pull Request ins
  CRM-Repository deployt (Workflow `sync-to-crm.yml`, bei Push auf `main`).
- `packages/` (`@mhvp/api-client`, `@mhvp/ui`, `@mhvp/config`), Lockfile und
  Workspace-Konfiguration sind **Spiegel** aus dem CRM-Repository. Änderungen daran gehören
  ins CRM-Repository und werden von dort übernommen (siehe unten), nie hier erfunden.

## Einrichtung des Sync (einmalig)

1. Im CRM-Repository ein Zugriffstoken mit Schreibrecht auf Inhalte und Pull Requests
   erstellen (fine-grained, nur für dieses eine Repository).
2. In diesem Repository unter Settings, Secrets and variables, Actions als Secret mit dem
   Namen `CRM_SYNC_TOKEN` hinterlegen. Den Wert nirgends im Code oder in Dateien ablegen.
3. Default-Branch dieses Repositories auf `main` setzen; der Sync läuft bei jedem Push auf
   `main` sowie manuell über `workflow_dispatch`.

## Entwicklung

Voraussetzungen: Node 22, pnpm 10.

```sh
pnpm install
pnpm --filter @mhvp/web-portal dev   # Portal auf http://localhost:3001
pnpm lint && pnpm typecheck && pnpm test
```

Das Portal erwartet die CRM-API; lokal läuft sie über das Hauptrepository
(`make dev`, `http://api.localhost`). Details: README und Runbooks im CRM-Repository.

## Spiegel aktualisieren (Pakete aus dem CRM übernehmen)

Bei Änderungen an `packages/*` oder dem API-Client im CRM-Repository:

```sh
rsync -a --delete <crm-checkout>/packages/ packages/
pnpm install --no-frozen-lockfile
pnpm typecheck && pnpm test
```

Anschließend als eigener Commit (`chore(mirror): packages aus CRM übernehmen`) einchecken.
