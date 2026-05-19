[update-readmes]   Mode: rewrite — migrating to template structure...
# linux-powerwash

[![Built with Ona](https://ona.com/build-with-ona.svg)](https://app.ona.com/#https://github.com/Interested-Deving-1896/linux-powerwash)

<!-- AI:start:what-it-does -->
This project provides a distro-agnostic and filesystem-agnostic tool for performing factory resets on Linux systems. It supports multiple reset modes, such as soft, medium, and hard resets, and includes plugins for handling specific distributions, filesystems, and hardware configurations. System administrators and power users utilize this tool to restore Linux systems to a clean state while preserving flexibility across environments.
<!-- AI:end:what-it-does -->

## Architecture

<!-- AI:start:architecture -->
The project consists of several components organized into distinct directories. The `bin` directory contains the main executable script (`powerwash`) responsible for initiating the factory reset process. The `lib` directory includes shared shell libraries for handling common tasks, such as filesystem operations, backup management, and plugin integration. The `modes` directory defines reset modes (e.g., soft, medium, hard) as modular scripts. The `plugins` directory provides optional extensions for specific distributions, filesystems, or hardware. Systemd integration is handled via the `systemd` directory, which includes service files and helper scripts. Configuration files are stored in `/etc/powerwash`, and logs are written to `/var/log`.

Directory structure:
```plaintext
.
├── bin
│   └── powerwash
├── lib
│   ├── common.sh
│   ├── distro.sh
│   ├── filesystem.sh
│   ├── backup.sh
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
├── docs
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
The repository uses GitHub Actions for Continuous Integration (CI). The following workflows are defined:

1. **`mirror-osp-to-ooc.yaml`**  
   - **Purpose**: Mirrors the repository to an external Git hosting service.  
   - **Triggers**: Runs on every push to the `main` branch.  
   - **Required Secrets**:  
     - `MIRROR_REPO_URL`: URL of the external repository.  
     - `MIRROR_REPO_TOKEN`: Authentication token for the external repository.

2. **`trigger-artifact-mirror.yml`**  
   - **Purpose**: Builds the project and uploads artifacts for mirroring.  
   - **Triggers**: Runs on pull requests and pushes to the `main` branch.  
   - **Required Secrets**: None.  

Both workflows ensure code quality and facilitate external repository synchronization.
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
[@Interested-Deving-1896](https://github.com/Interested-Deving-1896) - 5 commits  

*Note: This repository is a mirror. Please refer to the upstream source for additional contributions and updates.*
<!-- AI:end:contributors -->

## Origins

<!-- AI:start:origins -->
_Original project — no upstream fork._
<!-- AI:end:origins -->

## Resources

<!-- AI:start:resources -->
_No additional resource files found._
<!-- AI:end:resources -->

## License

<!-- AI:start:license -->
<!-- License not detected — add a LICENSE file to this repo. -->
<!-- AI:end:license -->
