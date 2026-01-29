# Reverse Proxy (Traefik) Integration

This suite expects reverse proxy routing to be handled externally (e.g., Traefik in `homelab-infra`).

## 1. External Network

All proxied services must attach to the shared Docker network:

```yaml
- proxy
```

Created once per host:

```bash
docker network create proxy
```

## 2. Traefik Labels

Each service exposes its UI via labels such as:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.radarr.rule=Host(`${RADARR_HOST}`)"
  - "traefik.http.routers.radarr.entrypoints=websecure"
  - "traefik.http.routers.radarr.tls.certresolver=le"
```

## 3. Environment Variables

All hostnames are provided via .env or injected at deploy time:

```
SONARR_HOST=sonarr.example.com
RADARR_HOST=radarr.example.com
...
```

## 4. HTTPS / Certificates

TLS certificates are handled by Traefik’s ACME integration.
No certificate files are stored in this repo.

## 5. Common Troubleshooting

- Ensure DNS points to the proxy host.
- Ensure the service is attached to the proxy network.
- Ensure service listens on the correct internal port.
- Ensure labels are correct and router names are unique.
