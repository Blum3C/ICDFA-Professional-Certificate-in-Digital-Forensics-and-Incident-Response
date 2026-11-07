# Disk and Partition Forensics Lab: Sector Analysis, Partition Tables and Boot Process

**Course:** CIP-B101 (Digital Forensics) | **Date:** November 2026 | **Author:** Boluwatife Adeyeye

A hands-on lab where I analysed how disks are structured at the sector level, inspected partition tables without altering the evidence, and traced the boot process from power-on to OS control. This is part of my ongoing, public learning journey in digital forensics.

---

## At a Glance

| | |
|---|---|
| **Focus** | Disk structure, partition analysis, file systems, boot process |
| **Environment** | Windows 10 (VirtualBox VM) and Kali Linux |
| **Tools** | PowerShell, DiskPart, Disk Management, `msinfo32`, Task Manager, Kali Linux CLI |
| **Approach** | Read-only inspection to preserve evidence integrity |

## Skills Demonstrated

- **Disk and partition analysis:** located partition boundaries by calculating start sectors, sector counts and sizes from raw byte offsets.
- **Evidence integrity:** used read-only inspection methods on both Windows and Linux.
- **Cross-platform work:** carried out the same analysis with native Windows tools and Kali Linux.
- **Process investigation:** examined running processes (name, PID, purpose) and explained why they matter in malware analysis.
- **Boot process knowledge:** explained BIOS/MBR versus UEFI/GPT boot flows, including where an investigator can find artefacts.
- **File system identification:** distinguished NTFS and CDFS volumes and explained how file systems guide data recovery.
- **Verification:** cross-checked my calculations against OS-reported values and confirmed the partition layout was contiguous.

## What I Did

1. **Identified the system and its storage:** captured hardware and OS details, and listed disks, partitions and volumes.
2. **Investigated processes:** reviewed live processes and documented what each one does.
3. **Calculated disk geometry:** worked out total sectors, bytes, KB, MB and GB from cylinder/head/sector values.
4. **Analysed my own disk:** computed start sector, sector count and size for each partition.
5. **Inspected partition tables safely:** used Disk Management, DiskPart, PowerShell and Kali Linux without modifying data.
6. **Explained the boot process:** documented the eight stages from power-on to kernel control.
7. **Identified file systems:** recorded NTFS and CDFS volumes and their forensic relevance.

## Sample Result

Partition layout on my VM disk (512-byte sectors, NTFS):

| Partition | Start Sector | Size | Sector Count |
|-----------|--------------|------|--------------|
| 1 | 2,048 | 50 MB | 102,400 |
| 2 | 104,448 | 50.32 GB | 105,523,200 |
| 3 | 105,627,648 | 450 MB | 921,600 |

The end of Partition 2 lines up exactly with the start of Partition 3, which confirms the calculations.

## Why It Matters in Digital Forensics

- Knowing exactly where partitions start and end tells an investigator where data can be found and recovered.
- Process information can reveal what an attacker was doing on a system.
- Identifying the file system determines which recovery tools and techniques apply.
- Read-only methods keep evidence admissible and unaltered.

## Links

- **Full lab report:** see `CIP-B101_Lab1B_Boluwatife_Adeyeye.pdf` in this repo
- **Screenshots:** see the `screenshots/` folder
- **LinkedIn:** https://linkedin.com/in/boluwatife-adeyeye

---

*Final-year Computer Engineering student at Obafemi Awolowo University with a focus on cybersecurity and digital forensics. Open to connecting and to feedback.*
