# homelab-ansible

Ansible playbooks that provision my homelab: an Arch Linux workstation, a Debian server, an old T480 laptop, and the k3s cluster running across the workstation and server.

Three hosts in the inventory — `main` (192.168.1.136, the Arch workstation), `t480` (192.168.1.134, an Arch laptop), and `server1` (192.168.1.137, Debian). To run something, point ansible-playbook at the right playbook: `playbooks/workstation.yml` for the Arch machines, `playbooks/server.yml` for the Debian box, `playbooks/k3s.yml` to install and join the cluster. Add `--tags server` (or whatever tag) if you only want part of it, e.g. `ansible-playbook playbooks/server.yml --tags server`.

Roles are split by what they touch. `common` runs on every machine and covers the basics — packages, timezone, hostname, SSH hardening. `workstation` and `server` handle the Arch-specific and Debian-specific setup, AUR packages and SDDM on one side, Docker/UFW/fail2ban and the compose templates on the other. `k3s` installs and joins the cluster.

Secrets aren't sitting in plaintext anywhere. They're SOPS-encrypted under `inventory/host_vars/*.sops.yml`, and `ansible.cfg` has the `community.sops.sops` vars plugin wired up so they decrypt automatically when a playbook runs. To actually run any of this you need SSH access and passwordless sudo on the target hosts, plus `ansible`, `sops`, and the `community.sops` collection installed locally.
