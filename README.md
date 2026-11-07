# linux-mint-disaster-recovery
Hands-on Linux Mint disaster recovery project documenting EXT4 filesystem failure, NVMe diagnostics, data backup, OS reinstallation, and system restoration.
# Linux Mint Disaster Recovery and System Restoration

## Project Overview

This project documents a hands-on Linux disaster recovery experience involving an EXT4 filesystem failure on a Linux Mint workstation.

The incident began when APT package management operations failed because the filesystem had become read-only. Further investigation revealed EXT4 journal errors and a protective read-only remount.

To recover the system, I used a Linux Mint Live USB to investigate the NVMe SSD, preserve important files and application configurations, and prepare for a fresh operating system installation.

After installing Linux Mint 22.3, I restored my development environment, verified personal files, reinstalled security tools, and tested system functionality.

## Project Objectives

- Investigate Linux filesystem and package management errors.
- Identify the cause of read-only filesystem behavior.
- Preserve important files and configurations before reinstalling Linux.
- Verify backup integrity using checksums.
- Restore essential applications and development tools.
- Implement automated malware scanning using ClamAV and systemd.
- Validate filesystem and SSD health after recovery.
- Document troubleshooting procedures, results, and lessons learned.

## Technologies and Tools

**Operating Systems:** Linux Mint 21.3, Linux Mint 22.3, Linux Mint Live USB

**Linux Utilities:** Bash, APT, journalctl, findmnt, rsync, smartctl, nvme-cli, tune2fs, systemd

**Backup and Security:** Samsung T7 external SSD, SHA-256, GPG encryption, ClamAV

**Restored Applications:** Anaconda, Python, Jupyter, Spyder, Visual Studio Code, Ollama, Bitwarden, Cisco Packet Tracer

## Recovery Outcome

The workstation was successfully rebuilt with Linux Mint 22.3.

Important personal files and development projects were restored, and essential applications were reinstalled and tested.

Post-recovery diagnostics confirmed that the EXT4 filesystem was clean and mounted read-write. The NVMe SSD passed its SMART overall-health assessment and reported zero media and data integrity errors.

The SSD continued to report NVMe invalid-command errors, so the original failure's underlying cause remains unconfirmed.

ClamAV malware detection and automated daily scanning were also successfully tested.

**Project scope:** Wazuh SIEM restoration is deferred to a separate project.
