# Media Suite Overview

This documentation describes the architecture, paths, and expectations for the **media automation suite**, including:

- Sonarr, Radarr, Lidarr, Readarr
- Prowlarr
- Bazarr
- Tautulli
- Ombi
- Unpackerr
- Slskd

These services work together to provide:

- Automated content acquisition
- Unified indexer management
- Automated subtitle handling
- Centralized media monitoring and requests
- Automated extraction and post-processing
- Consistent file permissions and directory structure


## High-Level Architecture
```
Download Clients
│
▼
Unpackerr → Media Managers (Sonarr/Radarr/Lidarr/Readarr)
│                   │
▼                   ▼
Completed Files     Metadata & DB
│                   │
└──────→ Media Library (/srv/media)
```

Reverse proxying, TLS termination, and DNS routing are done *outside* this repo (e.g., Traefik in the `homelab-infrastructure` repo).

## Deployment Philosophy

- This repo contains **only configuration** — no runtime data.
- All persistent data lives on the host under `/srv/servarrs/`.
- Updates are performed by committing changes to Git and redeploying.
- Environment variables and secrets are injected by your orchestrator.