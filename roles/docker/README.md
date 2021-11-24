# Docker
This role manages the installation of both Docker and Docker Compose. For Fedora hosts only.

## What it does
- Installs Docker Community Edition using the [repository](https://docs.docker.com/engine/install/fedora/#install-using-the-repository) method.
- Installs docker-compose following [this](https://docs.docker.com/compose/cli-command/#install-on-linux) instructions.

## Variables
- **docker_url**: url for the docker keys and repos
- **docker_gpg_key**: docker url subfolder that holds the repo keys
- **docker_repo**: docker url subfolder that holds the repo files
- **docker_compose_version**: version of Docker Compose to be installed
- **docker_compose_url**: url for the Compose binary
- **docker_compose_path**: path on the target host where Compose should be installed. `~/.docker/cli-plugins` for single user and `/usr/local/lib/docker/cli-plugins` for all users.

### Notes:
- Docker is installed and enabled as a service but no users are added to the docker group. You can check [here](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user) if you want access to the docker socket without `sudo`.
- The docker-compose v2 is being installed, if you wish to revert to a v1 version, don't forget to also change the variable *docker_compose_path* to `/usr/local/bin`.
- This role is a poor's man adaptation of the awesome [ansible-role-docker](https://github.com/geerlingguy/ansible-role-docker) from Jeff Geerling. Check it out for a much more robust solution that can be used on both Debian and RHEL based systems.
