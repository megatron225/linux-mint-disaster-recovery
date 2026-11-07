# Linux Mint Live USB Backup and Recovery

## 1. Recovery Environment

After identifying the EXT4 journal failure, I used a separate Windows 11 computer to prepare a bootable Linux Mint 22.3 USB drive using Rufus.

The Live USB allowed me to investigate the affected system without booting into the damaged Linux installation.

**Recovery equipment:**
- Linux Mint 22.3 Live USB (8 GB)
- Kingston 1 TB NVMe SSD (affected internal drive)
- Samsung T7 1 TB external SSD (backup destination)

## 2. Identifying Storage Devices

I used Linux command-line utilities to identify the available storage devices and their partitions.

```bash
lsblk -f
df -h
```

The affected Kingston drive contained an EFI partition and an EXT4 root partition.

The Samsung T7 external SSD used the exFAT filesystem and provided sufficient available storage for the prioritized backups.

Verifying the storage devices was essential to avoid accidentally modifying the wrong drive.

## 3. Prioritizing Data Preservation

Before reinstalling the operating system, I identified the files and configurations that needed to be preserved.

**High-priority data:**
- Personal documents and coursework
- Python projects and development files
- Wazuh SIEM configuration files
- Application settings and selected environment configurations
- Sensitive credential-related data requiring encryption

Large or easily reinstallable applications were given lower priority to conserve backup space and reduce recovery time.

## 4. Backing Up Files with rsync

I used `rsync` to copy important files from the affected filesystem to the external SSD.

An example of the backup command structure is:

```bash
sudo rsync -avh --progress \
  /path/to/mounted-root/home/USER/Documents/ \
  /path/to/external-drive/backup/Documents/
```

*The paths above are illustrative and use placeholders rather than personal directory names.*

The Documents backup transferred approximately 16.76 GB across 20,802 files.

### Backup complication: exFAT filename compatibility

The initial `rsync` operation returned exit code `23`, indicating that some files or attributes were not transferred successfully.

Investigation identified a filename containing a double-quote character that could not be created on the exFAT destination.

This demonstrated that a backup can transfer most files successfully while still reporting an incomplete operation.

The issue was documented rather than treating the backup as fully successful based only on the amount of data copied.

## 5. Preserving Application Configurations

I also backed up selected application configurations, including Wazuh dashboard, manager, and indexer configuration files.

These backups were retained for a separate Wazuh SIEM restoration project.

Other important development configurations and personal project files were preserved for recovery after the operating system reinstall.

## 6. Protecting Sensitive Information

Sensitive credential-related backups were stored in encrypted archives rather than as unencrypted files on the external exFAT drive.

GPG encryption was used for selected sensitive backup archives.

Private keys, passwords, authentication tokens, and credential backups are not included in this public repository.

## 7. Verifying Backup Integrity

After recovery, I used SHA-256 checksums and file comparisons to verify selected backup archives and restored files.

Example verification command:

```bash
sha256sum backup-file.tar.gz
```

Checksum comparisons helped verify that selected files had not changed during backup and restoration.

I also used file comparisons to validate restored development projects.

## 8. Lessons Learned

- A Live USB provides an independent environment for investigating an unbootable or unreliable Linux installation.
- Storage devices should be positively identified before running backup or repair commands.
- Backup priorities should be established before reinstalling an operating system.
- `rsync` exit codes must be reviewed rather than ignored.
- Filesystem compatibility can affect backup reliability.
- Sensitive data should be encrypted before being stored on removable media.
- Backups should be verified before relying on them for recovery.

## Outcome

Important personal files, development projects, and selected application configurations were preserved before the operating system was rebuilt.

The recovery process reinforced the importance of verified backups, careful troubleshooting, and protecting sensitive information throughout disaster recovery.


---

[← Previous: Incident Timeline](incident-timeline.md) | [Back to README](../README.md) | [Next: System Restoration →](system-restoration.md)
