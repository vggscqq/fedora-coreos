# Fedora CoreOS configs

* Edit your server config srv.bu
* Run docker compose up
* Get your builded Ignition file srv.ign.
* Install using `sudo coreos-installer install /dev/vda -i srv.ign`

## srv.bu
Customizations include enabling Docker, configuring networking, setting the system time zone, and installing additional tools.

* User Configuration:

    - Root User:
        - No SSH keys no password.
    - User vgscq:
        - Enabled sudo.
        - Password is `test`

* System Time Configuration:
    - Links /etc/localtime to Europe/Prague timezone.

* Storage and File Management:
    - /etc/hostname: hostname is `coreos`.
    - /etc/NetworkManager/system-connections/eth0.nmconnection: Configures the eth0 to DHCP and 8.8.8.8, 1.1.1.1 DNS.
    - /etc/selinux/config: SELinux to permissive mode.
    - /etc/sysconfig/nftables.conf: no firewall rules.
    - /etc/yum.repos.d/tailscale.repo: Adds Tailscale's repository.

* Systemd Service Units:
    - sshd.service: Ensures the SSH enabled and started.
    - docker.service: Ensures Docker is enabled and started.
    - postinst.service:
        A custom service for system initialization, which performs the following:
        - Runs once during the **first boot** to set system configurations and install packages.
        - Installs additional packages.
        - Remove nullok from system-auth.
        - `nvim` as a default editor.
        - Enables and starts the Tailscale service (tailscaled).
        - Reboots the system.

Shout out to https://github.com/twent/core-os-configs/tree/master
