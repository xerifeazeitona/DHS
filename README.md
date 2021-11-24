# Docker Traefik Home Server
This playbook spins a Home Media Server, with the following services:
- Traefik (reverse proxy)
- Authelia (multi factor authentication)
- Heimdall (application dashboard)
- Plex (media server)
- Nextcloud ("cloud" drive)
- UniFi Controller Software

## What it does
- Configure the target host via the [post install](roles/post_install) role.
- Installs Docker and Docker Compose via the [docker](roles/docker) role.
- Setup user, folders, secrets, environment variables, configuration and rules for Docker, Traefik and Authelia and also create a `docker-compose.yml` via the [stack](roles/stack) role.

## How to use it
- Have a fresh installed Fedora Server host (Vagrant box, virtual machine, bare metal, etc.)
- Clone the repo on a machine that has Ansible installed.
- Install the dependencies with `ansible-galaxy collection install -r requirements.yml`.
- Edit `inventory` and add the host IP on it.
- To enable the *automation* user, run `ansible-playbook setup_ansible_user.yml -u <USERNAME> -kK`, replacing `<USERNAME>` with a valid user with sudo privileges.
- Carefully review all variables and change their values according to your setup. 
- Run `ansible-playbook main.yml`
- Once it's done the host will be ready, just access it, navigate to the docker root folder and run `docker compose up -d`. 

## Important Notes
- This project is mostly an automated version of the [Ultimate Docker Home Server with Traefik 2, LE, and OAuth / Authelia](https://www.smarthomebeginner.com/traefik-2-docker-tutorial/), so check it out for very detailed, step-by-step instructions on how to build this stack. The main differences are that I'm using a Fedora Server instead of Ubuntu as host and DuckDNS instead of Cloudflare.
- The main focus of this playbook is to help with maintenance and redeployment. It's not recommended for a first-time run since some services (Plex and Nextcloud) require manual intervention for the initial setup.
- The use of an external drive is optional but highly recommended. With external mounts you can nuke and pave the server, run the playbook and be back up and running in less than 5 min, depending on how good your internet speed is.
