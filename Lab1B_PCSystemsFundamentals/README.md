# CIP-B101 Lab 1B: PC System Fundamentals, Disk Sectors and Partition Analysis

![Course](https://img.shields.io/badge/Course-CIP--B101-blue)
![Focus](https://img.shields.io/badge/Focus-Digital%20Forensics-informational)
![Platform](https://img.shields.io/badge/Platform-Windows%2010%20%7C%20Kali%20Linux-lightgrey)

Part of my ICDFA journey, documented in public. This lab covers how disks are organised into sectors and partitions, why that matters for digital forensics, and how to inspect partition tables safely on Windows and Linux.

| | |
|---|---|
| **Author** | Boluwatife Adeyeye |
| **Course** | CIP-B101 |
| **Instructor** | Aminu Idris |
| **Date** | 05/11/2026 |
| **Environment** | Windows 10 Home (VirtualBox VM), Kali Linux |

---

## Table of Contents

1. [Objectives](#objectives)
2. [Background](#background)
3. [Lab Structure](#lab-structure)
4. [Key Results](#key-results)
5. [Commands Reference](#commands-reference)
6. [Boot Process Summary](#boot-process-summary)
7. [Forensic Takeaways](#forensic-takeaways)
8. [Repository Layout](#repository-layout)
9. [Status](#status)

---

## Objectives

- Identify system specifications and map evidence sources to system layers.
- Observe programs, processes and threads, and understand their forensic value.
- Calculate disk and partition sizes from sector geometry.
- Inspect partition tables without modifying the disk (read-only).
- Explain the BIOS/MBR and UEFI/GPT boot process.
- Identify file systems and explain why they matter in investigations.

## Background

A **sector** is the smallest storage unit on a disk (512 bytes in this lab). Sectors are grouped into **partitions**, and the layout is recorded in a **partition table**, either **MBR** (Master Boot Record) or **GPT** (GUID Partition Table). In forensics, partition analysis lets an investigator locate where data begins and ends on a drive.

## Lab Structure

| Part | Topic | Summary |
|------|-------|---------|
| **A** | System identification and evidence layers | Captured system specs with `msinfo32` and `systeminfo`; storage details with PowerShell; mapped evidence to system layers |
| **B** | Programs, processes and threads | Observed processes via Task Manager and PowerShell; documented PID and purpose of selected processes |
| **C** | Disk sector size calculation | Reproduced the geometry calculation; computed offsets, sector counts and sizes for my own partitions |
| **D** | Safe partition table inspection | Disk Management, DiskPart and PowerShell on Windows; read-only inspection on Kali Linux |
| **E** | MBR, GPT and boot process | Explained the eight-stage boot sequence |
| **F** | File system identification | Identified NTFS and CDFS volumes and explained forensic relevance |
| **G** | Final practical assignment | Documented partition 1 and 2 offsets, sector counts and sizes, and compared with OS-reported values |

## Key Results

### C1. Geometry calculation

Given: 400 sectors/track, 12 heads, 17,000 cylinders, 512 bytes/sector, 1 KB = 1024 bytes.

| Item | Formula | Result |
|------|---------|--------|
| Total sectors | 400 x 12 x 17,000 | 81,600,000 sectors |
| Total bytes | 81,600,000 x 512 | 41,779,200,000 bytes |
| Kilobytes | bytes / 1024 | 40,800,000 KB |
| Megabytes | KB / 1024 | 39,843.75 MB |
| Gigabytes | MB / 1024 | 38.91 GB |

### C2. My VM disk (512-byte sectors, NTFS)

Start sector = offset / 512. Sector count = total bytes / 512.

| Partition | Offset (bytes) | Start sector | Size | Total bytes | Sector count | Size (GB) |
|-----------|---------------|--------------|------|-------------|--------------|-----------|
| 1 | 1,048,576 | 2,048 | 50 MB | 52,428,800 | 102,400 | 0.049 |
| 2 | 53,477,376 | 104,448 | 50.32 GB | 54,027,878,400 | 105,523,200 | 50.32 |
| 3 | 54,081,355,776 | 105,627,648 | 450 MB | 471,859,200 | 921,600 | 0.44 |

Sanity check: Partition 2's offset plus its size equals Partition 3's offset (53,477,376 + 54,027,878,400 = 54,081,355,776), so the layout is contiguous.

### F. File systems observed

- Partitions 1 and 2: **NTFS**
- VBox_GAs-6.1.50 volume: **CDFS** (read-only / write-once optical media)

## Commands Reference

**Windows**

```powershell
msinfo32                 # via Win + R
systeminfo
Get-Disk
Get-Partition
Get-Volume
Get-Process | Select-Object -First 20 Name,Id,CPU,WorkingSet
diskmgmt.msc             # via Win + R
diskpart                 # then: list disk / list partition
```

**Kali Linux (read-only inspection)**: see the screenshots in `screenshots/` for the exact commands and output.

## Boot Process Summary

1. **Power on:** the PSU signals the CPU, which starts executing firmware code at a fixed address.
2. **Firmware starts (BIOS/UEFI):** initialises CPU, memory controller and chipset.
3. **POST:** checks essential hardware; failures are signalled by beep codes or on-screen errors.
4. **CMOS/firmware settings:** boot order, date/time and hardware options are loaded.
5. **Boot device and boot records:** BIOS reads the MBR and runs the active partition's boot sector; UEFI reads the GPT and loads a `.efi` file from the EFI System Partition.
6. **Boot loader/manager:** on Windows, `bootmgr` reads the BCD and starts `winload` (GRUB's equivalent on Linux).
7. **Kernel loading:** `winload` loads the kernel, HAL, registry hive and boot-start drivers.
8. **OS control:** the kernel starts the session manager, services and login screen.

My VM uses **BIOS/legacy** boot.

## Forensic Takeaways

- **Partition tables** tell an investigator exactly where each partition starts and ends, which is essential for locating and carving data.
- **Process information** (names, PIDs, timestamps, purpose) helps identify suspicious activity during malware analysis.
- **File system identification** determines which tools and methods can recover files from a storage device.
- **Read-only inspection** preserves the integrity of the evidence.

## Repository Layout

```
CIP-B101-Lab1B/
├── README.md
├── screenshots/        # Evidence captures for Parts A to G
└── CIP-B101_Lab1B_Boluwatife_Adeyeye.pdf   # Full lab report
```

## Status

- [x] Parts A (A1, A2), B, C, D, E, G documented
- [ ] A3: evidence-to-layer mapping table (to be completed)
- [ ] F: Linux file system observation (to be completed)

---

*Part of my ICDFA learning journey. Feedback and discussion are welcome.*
