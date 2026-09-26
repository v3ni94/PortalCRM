# CLAUDE.md

Regeln für Claude Code in diesem Repository (PortalCRM).

## Kontext

- Dieses Repo ist das Entwicklungs-Repository für `apps/web-portal` der
  MH Verwaltungsplattform (Portal für Mieter, Eigentümer inkl. Beirat, Dienstleister).
- Hauptprojekt und einzige Quelle für API, Datenmodell und Pakete:
  `v3ni94/CRM-HV-Verwaltungssoftware`. Dessen `CLAUDE.md` und `docs/MASTER-PROMPT.md`
  gelten inhaltlich auch für Portal-Code (Gates, Mandantentrennung, keine erfundenen
  Rechtsregeln, KI nur als Vorschlag).

## Verbindliche Repo-Regeln

1. Entwickelt wird ausschließlich in `apps/web-portal/`. Jeder Merge nach `main` erzeugt
   über `.github/workflows/sync-to-crm.yml` automatisch einen Sync-PR im CRM-Repository.
2. `packages/`, `pnpm-lock.yaml` und die Workspace-Konfiguration sind Spiegel aus dem
   CRM-Repository. Nicht eigenständig fachlich ändern; bei Bedarf aus dem CRM übernehmen
   (Vorgehen im README) und als separater `chore(mirror)`-Commit einchecken.
3. Braucht eine Portal-Funktion neue API-Endpunkte oder Client-Methoden, entsteht diese
   Änderung im CRM-Repository, nicht hier. Hier nur dokumentieren, was benötigt wird.
4. Vor jedem Push: `pnpm lint && pnpm typecheck && pnpm test` lokal grün.
5. Conventional Commits. Keine Secrets im Code. Texte für Nutzeroberflächen auf Deutsch
   über `next-intl` (`apps/web-portal/messages/`).
