# homelab-ansible

Ansible playbooks for my homelab. The goal is that if I ever wipe either machine, I can run one command and get back to a fully configured state without touching anything manually.

Two machines:
- **main** (192.168.1.136) — Arch Linux, Hyprland desktop
- **server1** (192.168.1.137) — Debian 13, headless

## How to run it

Workstation:
```
ansible-playbook -i inventory/inventory.ini playbooks/workstation.yml
```

Server:
```
ansible-playbook -i inventory/inventory.ini playbooks/server.yml
```

To run only a specific role:
```
ansible-playbook -i inventory/inventory.ini playbooks/workstation.yml --tags workstation
```

## What each role does

**common** — runs on both machines. Hostname, timezone, base packages, SSH hardening (no root login, key-only auth).

**workstation** — Arch-specific stuff. Pacman packages, AUR packages via yay, enables SDDM/Bluetooth/auto-cpufreq, deploys SDDM config.

**server** — Docker, UFW firewall.

## Requirements

- SSH keys set up on both hosts
- Passwordless sudo on both hosts
- Ansible installed locally

