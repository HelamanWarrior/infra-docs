---
title: "Raspberry Pi"
---

Responsible for DNS, VPN, and SoulSeek. 

If the pi goes down internet access can be restored by [reconfiguring the router's NAT settings](posts/pihole-dns-setup).

The pi's configuration can be replicated with its [Ansible Playbook](https://github.com/HelamanWarrior/pi-server-playbook).

Port fowarding rules:

| port  | service    |
|-------|------------|
| 51820 | Wireguard  |
| 50300 | SoulSeek   |
