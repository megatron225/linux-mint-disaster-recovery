# Linux Mint System Restoration and Validation

## 1. Operating System Reinstallation

Following the data recovery process, I performed a fresh installation of Linux Mint 22.3 (Zena), replacing the previous Linux Mint 21.3 installation.

The objective was to establish a working operating system before restoring development applications, security utilities, and personal projects.

## 2. Filesystem and Storage Validation

After reinstalling Linux Mint, I performed additional checks to verify the filesystem and NVMe SSD.

### Root filesystem

```bash
findmnt -no SOURCE,FSTYPE,OPTIONS /
```

The root partition was mounted as EXT4 with read-write access.

Additional checks using `tune2fs` reported the filesystem state as `clean`.

### SSD health

```bash
sudo smartctl -H /dev/nvme0n1
sudo smartctl -A /dev/nvme0n1
```

The Kingston NVMe SSD passed its SMART overall-health assessment.

Reported measurements included:

| Measurement | Result |
|---|---|
| SMART overall-health assessment | PASSED |
| Available spare | 100% |
| Percentage used | 0% |
| Media and data integrity errors | 0 |
| Temperature | Approximately 30°C |

The NVMe error log continued to contain `Invalid Field in Command` entries.

These results did not establish a definitive cause for the original filesystem failure.

### Kernel log validation

I checked the current boot's kernel messages for additional EXT4 and NVMe errors.

```bash
sudo journalctl -k -b --no-pager | grep -Ei 'EXT4-fs.*(error|warning|abort|remount)|I/O error|nvme.*(reset|timeout)'
```

No matching messages were returned during that validation check.

## 3. Restoring the Development Environment

After confirming that Linux Mint was functioning, I restored and tested my primary development tools.

| Application | Validation |
|---|---|
| Visual Studio Code | Installed and tested with Python |
| Anaconda | Conda and Python environment verified |
| Spyder | Launched and tested |
| Jupyter | Functionality verified |
| Ollama | Local service and model responses tested |
| Bitwarden | Installed and authenticated |
| Google Chrome | Installed and synchronized |
| Cisco Packet Tracer 9.0.1 | Installed and tested with a basic LAN |

Selected personal projects and configuration files were restored from backups, with file comparisons used to validate recovery.

## 4. Security Tool Restoration

ClamAV was installed and configured for malware scanning.

I tested malware detection using the EICAR antivirus test file and verified that ClamAV successfully identified it.

A Bash script was created to scan the Documents and Downloads directories and save timestamped scan logs.

The script was integrated with a systemd user service and timer to schedule daily scans at 6:00 PM.

The scheduled service was observed executing successfully.

Detailed implementation of this security automation will be maintained in a separate ClamAV project.

## 5. Cisco Packet Tracer Network Validation

Cisco Packet Tracer 9.0.1 was installed from a Cisco Networking Academy package.

A basic LAN topology was created using:

- Two PCs
- One Cisco 2960 switch
- Ethernet connections

ICMP connectivity testing returned four successful replies out of four packets, with 0% packet loss.

The working topology was saved as `lab-01-basic-lan.pkt` for a separate networking portfolio project.

## 6. Deferred Components

Wazuh SIEM configuration backups were preserved during recovery.

Reinstallation and restoration of Wazuh are deferred to a separate project so the disaster recovery documentation can remain focused on the operating system recovery.

## 7. Recovery Outcome

The Linux Mint workstation was successfully rebuilt, and essential applications and personal projects were restored.

Post-recovery validation confirmed:

- An EXT4 filesystem reported as clean and mounted read-write.
- A passing NVMe SMART overall-health assessment.
- Working Python and development applications.
- Functional local AI tools.
- Successful ClamAV malware detection and scheduled scan execution.
- Successful basic LAN connectivity testing in Cisco Packet Tracer.

Although the system was restored successfully, the original cause of the EXT4 journal failure remains undetermined.

## Lessons Learned

This recovery reinforced the importance of verifying system health after restoration rather than assuming that a successful installation means every component is functioning correctly.

Testing individual applications, checking kernel logs, validating backups, and documenting unresolved findings were all important parts of the recovery process.
