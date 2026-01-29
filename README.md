# homelab-media-suite

This repository contains the **media automation ecosystem**, centered around:

- Prowlarr (indexer aggregator)
- Sonarr (TV)
- Radarr (Movies)
- Lidarr (Music)
- Readarr (Books) TODO: replace
- Bazarr (Subtitles)
- Tautulli (Plex monitoring)
- Overseerr (Media requests)
- Unpackerr (Post-processing)
- Slskd (Soulseek daemon)
- Future download clients

All services in this suite share:

- Common directory layout for config and media paths
- Shared external Docker network: `proxy`
- Optional Traefik reverse proxy configuration
- Centralized host-side data under `/srv/servarrs/<service>/config`
- GitOps workflow where this repo defines configuration only


## Repository Layout

```
docs/               → Documentation for running this suite
stacks/media-suite/ → Compose files and shared YAML definitions
.env.example        → Example variables needed for deployment
common.yaml         → Shared YAML anchors (PUID/PGID/TZ/etc.)
```


## Deployment expectations

- No runtime data lives in this repo.
- All persistent data is mounted under:

```
/srv/servarrs/<service>/config
/srv/media # except on my WSL host.
/srv/downloads # except on my WSL host.
```
- Secrets (hostnames, API keys) are not stored here — they are injected at deployment time by the orchestrator (currently Komodo).


## GitOps workflow

1. Edit config in Git.
2. Push changes.
3. The orchestrator pulls and redeploys the updated stack.

This keeps deployments **reproducible**, **atomic**, and **version-controlled**.