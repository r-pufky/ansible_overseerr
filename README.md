# Overseerr
Overseerr install from source on Debian.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_overseerr/blob/main/meta/main.yml)

[collections/roles](https://github.com/r-pufky/ansible_overseerr/blob/main/meta/requirements.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_overseerr/tree/main/defaults/main)

### Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_overseerr/blob/main/defaults/main/ports.yml)

## Dependencies
Part of the [r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv)
collection.

## Example Playbook
Read through defaults before using. Assumes you are already managing the debian
system. You must use your own information for configuring the service.

host_vars/overseerr.example.com/vars/settings.yml
``` yaml
overseerr_plex_name: 'my plex server'
overseerr_plex_ip: '127.0.0.1'
overseerr_plex_port: 32400
overseerr_plex_machine_id: '{PLEX PROCESSED MACHINE ID}'
overseerr_client_id: '{{ vault_overseerr_client_id}}'  # UUIDv5
overseerr_config_api_key: '{{ vault_overseerr_api_key }}'  # UUIDv4 base64

overseerr_plex_radarr:
  - name: 'radarr'
    hostname: '127.0.0.1'
    port: 7878
    api_key: '{RADARR API KEY}'
    use_ssl: false
    active_profile_id: 1
    active_profile_name: 'defined profile'
    active_directory: '/data/media'
    is_4k: false
    minimum_availability: 'released'
    tags: []
    is_default: true
    external_url: ''
    url_base: ''
    sync_enabled: true
    prevent_search: true
    tag_requests: false
    id: 0

overseerr_plex_sonarr:
  - name: 'sonarr'
    hostname: '127.0.0.1'
    port: 8989
    api_key: '{SONARR API KEY}'
    use_ssl: false
    active_profile_id: 1
    active_profile_name: 'defined profile'
    active_directory: '/data/media'
    active_language_profile_id: 1
    active_anime_profile_id: 1
    active_anime_profile_name: 'defined profile'
    active_anime_directory: '/data/media'
    active_anime_language_profile_id: 1
    tags: []
    anime_tags: []
    is_4k: false
    is_default: true
    enable_season_folders: true
    external_url: ''
    url_base: ''
    sync_enabled: true
    prevent_search: true
    tag_requests: false
    id: 0
```

host_vars/overseerr.example.com/vars/users.yml
``` yaml
overseerr_users:
  - id: 1
    comment: 'my admin account'
    email: 'admin_user@example.com'
    username: 'My Overseer Admin User'
    plex_id: 999999
    plex_token: '{PLEX AUTH TOKEN}'
    permissions: 2
    avatar: 'https://plex.tv/users/aasdfasdfas122/avatar?c=193845345'
    user_type: 1
    plex_username: 'PlexUserName'
    settings:
      watchlist_sync_movies: true
      watchlist_sync_tv: true
  - id: 2
    comment: 'Friend local access'
    email: 'local_user@example.com'
    password: '{PASSWORD}'
    permissions: 8388768
    user_type: 2
    settings:
      notification_types:
        pushbullet: 16
      watchlist_sync_movies: true
      watchlist_sync_tv: true
  - id: 3
    comment: 'Friend remote access'
    email: 'plex_user@example.com'
    permissions: 8388768
    user_type: 1
    settings:
      watchlist_sync_movies: true
      watchlist_sync_tv: true
```

site.yml
``` yaml
- name: 'Overseerr Server'
  hosts: 'overseerr.example.com'
  become: true
  roles:
     - 'r_pufky.srv.overseerr'
```

The overseerr role can deploy config only changes after the first run, greatly
increasing deployment speed (useful for updating settings from configuration
and adding users). Apply config only changes during a run:

```bash
ansible-playbook site.yml --tags Overseerr -e 'overseerr_service_force_config_only_enable=true'
```

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_srv/blob/main/docs/dev/environment/README.md)

Run all unit tests:
``` bash
molecule test --all
```

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_overseerr/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
