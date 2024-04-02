# Install from scratch
- backup existing db.
- comment ansible configs (only keep admin user and first normal user).
- clean system.
- verify service working
--> create backup image for testing state.
= role installs correctly and turns up service for use (can access w/o proxy).

# Version upgrade w/ migration=true
- use existing system from 'install from scratch'
- systemctl stop overseerr
- remove symlink
- add symlink with previous version (ln -s overseerr overseerr-v1.33.1)
# _ systemctl disable overseerr
# _ vim /etc/systemd/system/overseerr.service (bump version down)
# _ systemctl daemon-reload
# _ systemctl enable overseerr.service
# _ systemctl start overseerr.service
# - start service and validate it runs until old version
# - shutdown service
# --> create backup image for testing state.
- systemctl start overseerr
--> create backup image for testing state.
= role installs correctly and turns up service for use (can access w/o proxy); no user changes.

# Version upgrade w/o migration=false
- use existing system from 'install from scratch'
- UI: change admin user properly (display name)
- ansible: comment first normal user out
- ansible: set overseerr_enable_migration: false
--> create backup image for testing state.
= role installs correctly and turns up service for use (can access w/o proxy)
= reauth to plex as it is a non-migrated install
= admin user reset
= first user does not exist
