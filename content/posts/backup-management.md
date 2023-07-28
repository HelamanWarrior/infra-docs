---
title: "💾 Backup Management"
date: 2023-06-28
---

## Creating Restic repository

`restic -r sftp:admin@192.168.1.40:/share/FreeNAS/ init`

This repository will be the centeral location for backups. The password is used for encryption.
So I created my Restic repository on my backup machine. Set a password. And I'm off to the races.

---

# Autorestic

[Autorestic](https://autorestic.vercel.app/) is a great wrapper to [restic](https://restic.net/). It just makes the commands much more simple and readable. It also makes it much easier to automate restic.

## 🛳 Installation

```bash
wget -qO - https://raw.githubusercontent.com/cupcakearmy/autorestic/master/install.sh | bash
```

## 🎛 Configuration 

Everything is configured in a configuration YAML file. Saved inside the user's home directory `~/.autorestic.yml`.

```yml
version: 2

locations:
  zpool:
    from: /zfspool
    to: qnap-nas

  home:
    from: /home/parker
    to: qnap-nas

backends:
  qnap-nas:
    type: sftp
    path: admin@192.168.1.40:/share/FreeNAS/
    key: your-vault-encryption-key
```

## 🔐 Root file permission backups

Restic is unable to backup files outside of the user's permissions. 

This poses issues with files created by Docker containers. Some Docker containers have very specific file privileges given to volumes. This leads to those files failing to get backed up.

The solution is to give the Restic binary access to these files.

```bash
sudo chown root:$(whoami) $(which restic)
sudo chmod 750 $(which restic)
sudo setcap cap_dac_read_search=+ep $(which restic)
```

## 💾 Backing up files

Backing up all the locations in the configuration file.

```bash
autorestic backup -a
```

## 📸 List all snapshots for all backends

```bash
autorestic exec -av -- snapshots
```

## ✨ Restore backup

```bash
autorestic restore -l zpool --from qnap-nas --to /zfspool
```

Restoring `zpool` from the `QNAP NAS` back to the machine.

```bash
autorestic restore -l home --from qnap-nas --to /home/parker
```

Restoring docker files, scripts, and user files.
