[update-readmes]   Mode: rewrite — migrating to template structure...
# linux-powerwash

[![Built with Ona](https://ona.com/build-with-ona.svg)](https://app.ona.com/#https://github.com/Interested-Deving-1896/linux-powerwash)

<!-- AI:start:what-it-does -->
This project provides a distro-agnostic and filesystem-agnostic factory reset tool for Linux systems. It enables system administrators and advanced users to restore a Linux installation to a clean state using customizable reset modes and plugins. It supports various distributions and filesystems, making it suitable for diverse environments.
<!-- AI:end:what-it-does -->

## Architecture

<!-- AI:start:architecture -->
The project consists of several components organized into distinct directories. The main script (`bin/powerwash`) serves as the entry point, orchestrating the factory reset process. Shared functionality is implemented in modular shell libraries located in `lib/`. Reset modes, which define varying levels of system cleanup, are stored in `modes/`. Plugins in `plugins/` provide optional, extensible functionality for specific distributions, filesystems, or hardware. Systemd service files and helpers are located in `systemd/` for integration with system services. Configuration files are stored in `/etc/powerwash`, while logs and state data are maintained in `/var/log` and `/var/lib/powerwash`, respectively.

Directory structure:
```plaintext
.
├── bin
│   └── powerwash
├── docs
├── lib
│   ├── backup.sh
│   ├── common.sh
│   ├── distro.sh
│   ├── filesystem.sh
│   └── plugin.sh
├── modes
│   ├── soft.sh
│   ├── medium.sh
│   ├── hard.sh
│   ├── sysprep.sh
│   └── hardware.sh
├── plugins
│   ├── distro
│   │   └── ubuntu-ppa.sh
│   ├── filesystem
│   │   └── btrfs-snapshot.sh
│   └── hardware
│       └── amd-gpu.sh
├── systemd
│   ├── powerwash-rebind.service
│   └── rebind-helper
├── .github
├── LICENSE
├── Makefile
└── README.md
```
<!-- AI:end:architecture -->

## Install

<!-- Add installation instructions here. This section is yours — the AI will not modify it. -->

```bash
git clone https://github.com/Interested-Deving-1896/linux-powerwash.git
cd linux-powerwash
```

## Usage


```
powerwash [--dry-run] <command> [options]
```

### Reset modes

```bash
# Clear user dotfiles only — safe, non-destructive
sudo powerwash soft

# Remove user-installed packages and reset dotfiles
sudo powerwash medium

# Full factory reset: packages, home dirs, system config
sudo powerwash hard

# Generalize for disk imaging (clears machine-id, SSH keys, network state)
sudo powerwash sysprep --shutdown
```

### Preview before committing

Every command supports `--dry-run`. Nothing is modified:

```bash
sudo powerwash --dry-run hard
```

### Backup

```bash
# Create a backup before resetting manually
sudo powerwash backup create

# Create an encrypted backup
sudo powerwash backup create --encrypt

# List backups
sudo powerwash backup list

# Restore packages from a backup
sudo powerwash backup restore-packages /var/lib/powerwash/backups/backup_20240101_120000
```

### Filesystem snapshots

```bash
# Create a snapshot (btrfs or ZFS only)
sudo powerwash snapshot create my-label

# List snapshots
sudo powerwash snapshot list

# Roll back
sudo powerwash snapshot rollback my-label
```

### Hardware device reset

```bash
# List devices and their sysfs BUS IDs
sudo powerwash hardware list

# Rebind a USB controller after resume
sudo powerwash hardware rebind 0000:00:14.0

# Trigger AMD GPU vendor-reset (requires gnif/vendor-reset module)
sudo powerwash hardware vendor-reset 0000:01:00.0
```

### System info

```bash
powerwash info
```

### Interactive menu

```bash
sudo powerwash menu
```

---

## Configuration

<!-- Document configuration options here. This section is yours — the AI will not modify it. -->

## CI

<!-- AI:start:ci -->
- **trigger-artifact-mirror.yml**: Runs on `push` and `pull_request` events. Builds the project using the `Makefile` and uploads the generated artifacts for distribution. Requires the `ARTIFACTS_REPO_TOKEN` secret for authentication with the artifact repository.
<!-- AI:end:ci -->

## Mirror chain

<!-- AI:start:mirror-chain -->
This repo is maintained in [`Interested-Deving-1896/linux-powerwash`](https://github.com/Interested-Deving-1896/linux-powerwash) and mirrored through:

```
Interested-Deving-1896/linux-powerwash  ──►  OpenOS-Project-OSP/linux-powerwash  ──►  OpenOS-Project-Ecosystem-OOC/linux-powerwash
```

Changes flow downstream automatically via the hourly mirror chain in
[`fork-sync-all`](https://github.com/Interested-Deving-1896/fork-sync-all).
Direct commits to OSP or OOC are detected and opened as PRs back to `Interested-Deving-1896`.
<!-- AI:end:mirror-chain -->

## Contributors

<!-- AI:start:contributors -->
[@Interested-Deving-1896](https://github.com/Interested-Deving-1896): 3 commits  

*Note: This repository is a mirror. Please refer to the upstream source for the original project.*
<!-- AI:end:contributors -->

## Origins

<!-- AI:start:origins -->
_No dependency graph found. Run `generate-dep-graph.yml` to generate `dep-graph/origins.md`._
<!-- AI:end:origins -->

## Resources

<!-- AI:start:resources -->
_No additional resource files found._
<!-- AI:end:resources -->

## License

<!-- AI:start:license -->
<!-- License not detected — add a LICENSE file to this repo. -->
<!-- AI:end:license -->
