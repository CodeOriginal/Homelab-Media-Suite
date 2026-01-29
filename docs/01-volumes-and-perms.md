# Volumes & Permissions

Correct volume layout and permissions are essential for proper operation of the Servarr ecosystem.

## 1. Host Directory Layout

Recommended host-side directories:
```
/srv/servarrs/<service>/config    (service configuration files)
/srv/media                        (final media library)
/srv/downloads                    (download client output dirs)

Examples:

- `/srv/servarrs/sonarr/config`
- `/srv/servarrs/radarr/config`
- `/srv/servarrs/prowlarr/config`

## 2. Why This Layout

- Ensures consistent paths across services
- Simplifies Docker Compose volume definitions
- Prevents permission conflicts
- Makes backup/restore straightforward
- Keeps runtime data out of Git

## 3. File Ownership (PUID/PGID)

Most LinuxServer.io containers (Sonarr/Radarr/etc.) require:

```
PUID=1000
PGID=1000
```
These should match the user that owns your `/srv` directory tree.

You can check your user ID with:

```bash
id $USER
```

## 4. Permissions

Set directory ownership once:

```shell
sudo chown -R 1000:1000 /srv/servarrs
sudo chown -R 1000:1000 /srv/media
sudo chown -R 1000:1000 /srv/downloads
```
## 5. Common Volume Mounts

Example (Sonarr):

```yaml
volumes:
  - /srv/servarrs/sonarr/config:/config
  - /srv/media:/media
  - /srv/downloads:/downloads
```

All related services should share these paths consistently.
