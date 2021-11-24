# Fedora post install
This role does the post install configuration on Fedora Server hosts. Only tested with versions 34 and 35.

## What it does
### DNF Flags
- Enable `deltarpm` and `fastestmirror`
- Set `max_parallel_downloads` to **10** (default is 3)

### Repository setup
- Enable RPM Fusion repos (free and nonfree)
- Disable `fedora-modular`, `fedora-cisco-openh264` and `*-testing` repos
- Disable machine counting on all repos

### Package management
- Update the whole system to latest
- Install, start and enable the automatic updates service (`dnf-automatic`)
- Install extra packages (defined in `extra_packages` variable)
- Remove unnecessary packages installed as dependencies but are no longer required (`dnf autoremove`)

### User management
- Create a user with sudo privileges and a private key
- Copy tmux configuration to user home directory
- Add xclip aliases to user `.bashrc`

## Variables
- **hostname** and **domain**: hostname and domain of the machine. The script will concatenate both to create the machine FQDN
- **user_name**: name of the user to be created
- **user_pass**: password of the user. Note: to set the password you only need to change the text inside the `''` (*cleartext* in the default)
- **user_groups:** comma separated list of groups the user will be added to.
- **extra_packages:** list of packages to be installed 
