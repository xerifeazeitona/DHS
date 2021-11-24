# Stack
This role manages the configuration of Docker, Traefik and Authelia. It also generates a `docker-compose.yml` with the entire stack.

## What it does
### Docker
- Create Docker user/Enable existing user to run docker commands without `sudo`.
- Ensure docker folders (docker root, secrets, data and appdata) exist, with the ability to mount an external drive for data and appdata folders.
- Add extra permissions (ACL) to docker group on docker folders
- Generate a `.env` file with the environment variables necessary for the stack to run properly

### Traefik
- Ensure Traefik folders exist
- Ensure `traefik.log` exists
- Ensure `acme.json` exists
- Generate all the middleware and chain files necessary for the stack to run properly

### Authelia
- Ensure Authelia folders exist
- Ensure notification file exist
- Generate Authelia configuration files

### Compose file
- Generate all the necessary secret files
- Generate the `docker-compose.yml` file based on what has been provided by the variables.

## Variables
### External drive configuration
- **use_external_drive**: determines if an external drive should be used for the data and appdata folders. If set to *false* the folders will be created normally under the docker root folder. If set to *true* the external drive will be mounted, the folders will be created there and symlinks will be created under the docker root folder for ease of management.
- **external_drive_path**: path on the target host where the drive should be mounted
- **external_drive_uuid**: uuid of the external drive
- **external_drive_fstype**: filesystem of the external drive

*note:* The mount is persistent (it adds a line to the `/etc/fstab` of the target host)

### Docker configuration
**docker_user**: user that will be running docker commands. User will be created if it doesn't exist.
**docker_pass**: password for the user (only necessary for new user)
**docker_root_path**: docker root folder path. Everything will be created under this folder.
**docker_data_path**: Optional folder to store any kind of data that aren't necessary for the containers to run, like metadata, album art, etc. The advantage is to have leaner backups of the appdata folder.
**docker_appdata_path**: Persistent data for all the services. Each service will have a subfolder under this one.
**docker_secrets_path**: This folder will hold all the secrets used in the Compose file.

### Environment variables
**timezone**: The timezone that will be used for all services.
**domain_name**: Your domain name. This will be used to configure the reverse proxy for the services with the format `service_name.domain_name`. e.g. *simple-service.example.com*
**email**: Used only for Let's Encrypt email notifications.

### Traefik configuration
**traefik_dir**: Traefik folder name.
**traefik_acme_dir**: Location for the `acme.json` file
**traefik_rules_dir**: All Traefik rules will go under this folder.

### Authelia configuration
**authelia_dir**: Authelia folder name
**authelia_db_name**: Name for Authelia's database
**authelia_db_username**: DB admin user for Authelia database
**authelia_user_name**: Actual Authelia user, the one that will login into the services using Authelia as auth.
**authelia_user_displayname**: Display name for the Authelia user
**authelia_user_password_hash**: Hashed password for the Authelia user. To create a hash use `docker run authelia/authelia:latest authelia hash-password YOUR_PASSWORD`
**authelia_user_email**: email for the Authelia user. Unecessary for our current setup since we didn't enable SMTP notifications.

### Compose file
#### Networks
**traefik_subnet**: network address for the traefik docker network
**socket_proxy_subnet**: network address for the socket_proxy docker network

#### duckdns
**duckdns_domain**: your domain on duckdns, *without* the `.duckdns.org` part
**duckdns_dir**: duckdns folder name

#### Traefik proxy
**traefik_version**: Traefik version (currently using **brie**)
**traefik_static_ip**: static IP for Traefik service. Must be in the same network provided in **traefik_subnet**
**traefik_subdomain_name**: subdomain for Traefik dashboard

#### Socket Proxy
**socket_proxy_static_ip**: static IP for Socket Proxy service. Must be in the same network provided in **socket_proxy_subnet**

#### Authelia
**authelia_version**: Authelia version. Even though it works with latest, breaking changes have been introduced in the past so it's best to check the release before changing this.
**authelia_subdomain_name**: subdomain for Authelia login page

#### Heimdall
**heimdall_dir**: Heimdall folder name

#### Plex Media Server
**plex_dir**: Plex folder name
**plex_static_ip**: static IP for Plex. Must be in the same network provided in **traefik_subnet**
**plex_version**: Plex version. You need a Plex Pass subscription in order to use *latest*.
**plex_subdomain_name**: subdomain for Plex
**media_folder_path**: Path for your root media folder
**plex_media_folders**: List of folders under your media_folder_path that should be exposed to Plex

#### Nextcloud
**nextcloud_dir**: Nextcloud folder name
**nextcloud_subdomain_name**: subdomain for Nextcloud

#### UniFi Controller Software
**unifi_controller_dir**: UniFi folder name
**unifi_controller_subdomain_name**: subdomain for UniFi dashboard
