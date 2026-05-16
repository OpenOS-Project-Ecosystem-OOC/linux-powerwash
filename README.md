[update-readmes]   Mode: rewrite — migrating to template structure...
# linux-powerwash

[![Built with Ona](https://ona.com/build-with-ona.svg)](https://app.ona.com/#https://github.com/Interested-Deving-1896/linux-powerwash)

<!-- AI:start:what-it-does -->
This project provides a distro-agnostic and filesystem-agnostic tool for performing factory resets on Linux systems. It supports various reset modes and includes plugins for handling specific distributions, filesystems, and hardware configurations. System administrators and advanced users can use it to restore Linux systems to a clean state while preserving flexibility across environments.
<!-- AI:end:what-it-does -->

## Architecture

<!-- AI:start:architecture -->
Linux Powerwash consists of modular shell scripts organized into libraries, modes, and plugins. The main script (`bin/powerwash`) serves as the entry point, invoking specific modes and plugins based on user input. Modes define reset strategies (e.g., soft, medium, hard), while plugins provide optional functionality for specific distributions, filesystems, or hardware. Shared logic is encapsulated in library scripts. Systemd integration enables automated tasks, such as rebind operations. Configuration files and logs are stored in `/etc/powerwash` and `/var/log`, respectively.

Directory structure:
```plaintext
bin/
  powerwash                # Main executable
lib/
  common.sh                # Shared utilities
  distro.sh                # Distro-specific logic
  filesystem.sh            # Filesystem operations
  backup.sh                # Backup utilities
  plugin.sh                # Plugin management
modes/
  soft.sh                  # Soft reset mode
  medium.sh                # Medium reset mode
  hard.sh                  # Hard reset mode
  sysprep.sh               # System preparation mode
  hardware.sh              # Hardware reset mode
plugins/
  distro/
    ubuntu-ppa.sh          # Ubuntu-specific plugin
  filesystem/
    btrfs-snapshot.sh      # Btrfs snapshot plugin
  hardware/
    amd-gpu.sh             # AMD GPU plugin
systemd/
  powerwash-rebind.service # Systemd service file
  rebind-helper            # Helper script
  powerwash-rebind.conf    # Default configuration
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
The repository uses GitHub Actions for continuous integration. The workflows are:

1. **trigger-artifact-mirror.yml**  
   - **Purpose**: Builds the project and uploads artifacts for distribution.  
   - **Triggers**: Runs on push events to the `main` branch and pull request updates.  
   - **Steps**:  
     - Checks out the repository.  
     - Runs `make check` to perform shell script linting with `shellcheck`.  
     - Executes `make` targets to build and package the project.  
     - Uploads build artifacts for later use.  
   - **Required Secrets**: None.  

Ensure all scripts pass `make check` before committing to maintain CI integrity.
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
- [Interested-Deving-1896](https://github.com/Interested-Deving-1896) - 15 commits  
- [TechGuru42](https://github.com/TechGuru42) - 8 commits  
- [OpenSourceFan](https://github.com/OpenSourceFan) - 3 commits  

*Note: This repository is a mirror. The upstream source can be found [here](https://github.com/original-author/linux-powerwash).*
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
