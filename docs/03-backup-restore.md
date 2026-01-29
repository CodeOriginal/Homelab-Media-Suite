# Backup & Restore

This document explains how to safely back up and restore the media automation suite.

## 1. What to Back Up

Do **not** back up anything in this Git repo.  
Back up host-side runtime data only:

```
/srv/servarrs//config
/srv/media
/srv/downloads
```

Where:

- **config/** contains DBs, XML configs, logs, state
- **media/** contains final library files
- **downloads/** contains recent downloads (optional to back up)

## 2. Backup Strategy

Recommended schedule:

- **Nightly**: config backups
- **Weekly**: media snapshots (if using ZFS/Btrfs)
- **Incremental**: for large media directories

Example using `rsync`:

```bash
rsync -avh /srv/servarrs/ /backups/servarrs/
rsync -avh /srv/media/ /backups/media/
```

## 3. Restore Procedure

1. Stop the stack:
```shell
docker compose down
```
2. Restore config directories to:
```
/srv/servarrs/<service>/config
```
3. Restore media (if needed):
```
/srv/media
```
4. Ensure correct permissions:
```bash
chown -R <user>:<group> /srv/servarrs/
```
5. Re-deploy the stack.

## 4. Testing Backups
Perform a full restore test on a stagin host every few months to avoid surprises. 
