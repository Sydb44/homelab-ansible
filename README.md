# homelab-ansible

Ansible for the homelab hosts and base service provisioning.

## Inventory

- `main` (`192.168.1.136`) — Arch Linux workstation
- `t480` (`192.168.1.134`) — Arch Linux laptop
- `server1` (`192.168.1.137`) — Debian server

## Playbooks

Workstations:

```bash
ansible-playbook playbooks/workstation.yml
```

Server:

```bash
ansible-playbook playbooks/server.yml
```

K3s cluster:

```bash
ansible-playbook playbooks/k3s.yml
```

Run a specific tag:

```bash
ansible-playbook playbooks/server.yml --tags server
```

## Roles

- `common` — shared packages, timezone, hostname, SSH hardening
- `workstation` — Arch workstation packages, AUR packages, SDDM, desktop services
- `server` — Debian server packages, Docker, UFW, fail2ban, compose templates
- `k3s` — K3s control-plane/worker installation

## Secrets

Host and group secrets are stored as SOPS files such as `inventory/host_vars/server1.sops.yml`.
Ansible decrypts them through the `community.sops.sops` vars plugin configured in `ansible.cfg`.

## Requirements

- SSH access to the hosts
- Passwordless sudo on the hosts
- `ansible`, `sops`, and the `community.sops` collection installed locally
