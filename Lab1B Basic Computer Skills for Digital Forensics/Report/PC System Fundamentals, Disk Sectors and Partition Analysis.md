# PC System Fundamentals, Disk Sectors and Partition Analysis

![Course](https://img.shields.io/badge/Course-CIP--B101-0A66C2?style=flat-square)
![Platform](https://img.shields.io/badge/Platforms-Windows%2011%20%7C%20Kali%20Linux-5C2D91?style=flat-square)
![Focus](https://img.shields.io/badge/Focus-Digital%20Forensics-2E8B57?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)



| Field | Details |
|---|---|
| **Student Name** |Ellie Ng'oma |
| **Registration Number** |C11/26/PCDFIR/17012 |
| **Course Code** | CIP-B101 |
| **Lab Title** | PC System Fundamentals, Disk Sectors and Partition Analysis |
| **Date** | 07 November 2026 |
| **Instructor Name** |Basiru Aliyu |

## Table of Contents

- [Introduction](#introduction)
- [Tools and Platforms](#tools-and-platforms)
- [Part A: System Identification and Evidence Layer Mapping](#part-a-system-identification-and-evidence-layer-mapping)
- [Part B: Program, Process and Thread Investigation](#part-b-program-process-and-thread-investigation)
- [Part C: Disk Sector and Disk Size Calculation](#part-c-disk-sector-and-disk-size-calculation)
- [Part D: Safe Partition Table Inspection](#part-d-safe-partition-table-inspection)
- [Part E: MBR, GPT and Boot Process Explanation](#part-e-mbr-gpt-and-boot-process-explanation)
- [Part F: File System Identification and Interpretation](#part-f-file-system-identification-and-interpretation)
- [Part G: Final Practical Assignment](#part-g-final-practical-assignment)
- [Conclusion](#conclusion)

## Introduction

This report examines key digital forensics concepts related to system processes, storage devices, partitions, file systems, and the boot process. Evidence is collected using **Disk Management**, **PowerShell**, **DiskPart**, **Task Manager**, and **Kali Linux command-line tools**. The findings are used to analyze system activity, verify partition sizes, and identify potential sources of digital evidence.

## Tools and Platforms

| Tool or Platform | Purpose |
|---|---|
| **Windows 11 Pro** | Host operating system examined during the practical work |
| **System Information and `systeminfo`** | Collection of hardware, firmware, and operating system information |
| **Task Manager** | Observation of active processes and resource use |
| **Disk Management** | Graphical inspection of disk layout and partitions |
| **PowerShell** | Collection of disk, partition, and volume information |
| **DiskPart** | Command-line verification of the Windows partition table |
| **Kali Linux** | Read-only inspection of Linux disks, partitions, and mounted file systems |
| **`lsblk`, `fdisk`, `parted`, `blkid`, and `df`** | Linux storage and file-system identification commands |

---

## Part A: System Identification and Evidence Layer Mapping

To identify computer system layers and to explain how each layer can produce forensic evidence.

### A1. Capture Basic System Specification

<img width="525" height="250" alt="Fig_01 System information" src="https://github.com/user-attachments/assets/b33029c9-e0e4-4a1b-a2b0-8a585f02a1a6" />


*Figure 1: System information.*

<img width="232" height="73" alt="Fig_02 RAM" src="https://github.com/user-attachments/assets/3c160b3d-fc9a-40bb-a34e-67d3797fe1dd" />


*Figure 2: RAM.*

<img width="272" height="19" alt="Fig_03 Secure boot" src="https://github.com/user-attachments/assets/d9d969bc-fb97-4fe8-9913-cc49f1ea3326" />


*Figure 3: Secure Boot.*

<img width="712" height="434" alt="Fig_04 Command prompt system info" src="https://github.com/user-attachments/assets/4d188659-974c-476e-a859-fb692424f5f0" />


*Figure 4: Command Prompt system info.*

### A2. Capture Storage Information

<img width="448" height="221" alt="Fig_05 Disk" src="https://github.com/user-attachments/assets/4cab60df-9edd-490e-8251-48432d0b88b8" />


*Figure 5: Disk.*

<img width="647" height="149" alt="Fig_06 Partition" src="https://github.com/user-attachments/assets/6f39663d-f61a-4719-9a58-3f6f0f3a16b5" />


*Figure 6: Partition.*

<img width="593" height="104" alt="Fig_07 Volume" src="https://github.com/user-attachments/assets/431d5692-f349-4957-b2ac-ec4c23054d53" />


*Figure 7: Volume.*

### A3. Map Evidence Sources to System Layers

**Table 1**

| Layer/Source | Example Evidence | Where to Be Found | Why It Matters |
|---|---|---|---|
| User Layer | User opened `Report.docx` | Recent Files list and Jump List | Directly links the user to the document activity. |
| Application Layer | Microsoft Word adds `Report.docx` to its Recent Documents list. | Microsoft Word recent files and Office metadata | Shows which application was used to access the document. |
| Operating System Layer | Windows records file access information for `Report.docx`. | Event Logs, Registry, and user profile | Helps establish when and by whom the document was accessed. |
| Disk Layer | File timestamps for `Report.docx` are updated. | NTFS file system, MFT, and disk partitions | Supports timeline analysis and recovery of deleted evidence. |
| Memory/Process Layer | `Report.docx` and `WINWORD.EXE` may be present in RAM. | RAM capture (memory dumps) and process lists | Shows what was actively running on the system at the time and may contain evidence not yet saved to disk. |

---

## Part B: Program, Process and Thread Investigation

### B1. Observe Running Processes

<img width="485" height="129" alt="Fig_08 Running processes" src="https://github.com/user-attachments/assets/cb216311-3a1e-4889-bc96-395ea0e9f271" />


*Figure 8: Running processes.*

**Table 2**

| Process Name | PID | Purpose |
|---|---:|---|
| `Copilotapp.exe` | 15308 | Runs the Microsoft Copilot application and provides AI assistant functionality on the system. |
| `dllhost.exe` | 10000 | COM Surrogate process that allows Windows to run software components and extensions outside the main application to improve stability. |
| `csrss.exe` | 796 | Client Server Runtime Process, a critical Windows system process responsible for console windows, thread management, and other core operating system functions. |

### B2. Explain the Difference

**Difference Between a Program, Process, and Thread**

- A program is software stored on a computer, such as Microsoft Word.
- A process is a program that is currently running.
- A thread is a smaller task within a process that allows work to be performed simultaneously.

**The reason why process information is useful**

Process information helps investigators identify what was running on a system at a specific time. During malware analysis and forensic triage, it can reveal suspicious processes, malware activity, and unusual system behaviour.

---

## Part C: Disk Sector and Disk Size Calculation

### C1. To Calculate Disk or Partition Size

<img width="608" height="243" alt="Fig_09 Disk partition size" src="https://github.com/user-attachments/assets/8b881a3e-e478-4790-888f-1d77e0ec097d" />


*Figure 9: Disk partition size.*

### Example 1

**Table 3**

| Item | Value |
|---|---|
| Partition | Partition 3 (C:) |
| Start Sector | 239,616 |
| Number of sectors | 497,563,648 |
| Sector size | 512 bytes |
| File system | NTFS |

**Calculations**

```text
Partition Size (GB) = Number of Sectors x 512 / 1024 / 1024 / 1024
                    = 497,563,648 x 512 / 1024 / 1024 / 1024
                    = 237.26 GB
```

### Example 2

**Table 4**

| Item | Value |
|---|---|
| Partition | Partition 4 (Recovery) |
| Start Sector | 497,803,264 |
| Number of sectors | 2,310,144 |
| Sector size | 512 bytes |
| File system | NTFS |

**Calculations**

```text
Partition Size (GB) = Number of Sectors x 512 / 1024 / 1024 / 1024
                    = 2,310,144 x 512 / 1024 / 1024 / 1024
                    = 1.10 GB
```

---

## Part D: Safe Partition Table Inspection

### D1. Windows Method - Disk Management, PowerShell and DiskPart

**Disk layout and visible partitions in Disk Management**

<img width="559" height="104" alt="Fig_10 Visible partition" src="https://github.com/user-attachments/assets/7715b26b-7fa3-4b3a-8fcd-3b01ad153391" />


*Figure 10: Visible partition.*

**In PowerShell**

<img width="448" height="221" alt="Fig_11 Disk" src="https://github.com/user-attachments/assets/7ce3f3dd-cf82-4ec3-8a35-ee130cbd5b29" />


*Figure 11: Disk.*

<img width="647" height="149" alt="Fig_12 Partition" src="https://github.com/user-attachments/assets/af4424b0-d46d-43cf-8bb4-d8816225ecdd" />


*Figure 12: Partition.*

<img width="593" height="104" alt="Fig_13 Volume" src="https://github.com/user-attachments/assets/0da7086e-da7f-4051-b6d1-acd2b022c8b8" />


*Figure 13: Volume.*

**DiskPart in Command Prompt**

<img width="353" height="385" alt="Fig_14 Diskpart" src="https://github.com/user-attachments/assets/7a254f15-276b-439b-9729-37cb3f071341" />


*Figure 14: DiskPart.*

### D2. Linux/Kali Method - Read-Only Inspection

<img width="451" height="263" alt="Fig_15 kali disk info" src="https://github.com/user-attachments/assets/776f316a-b4c2-4e05-a6b8-929c2f895330" />


*Figure 15: Kali disk info.*

<img width="623" height="263" alt="Fig_16 Kali parted" src="https://github.com/user-attachments/assets/2f1cc354-00fe-42ce-b2dd-3caf2038066c" />


*Figure 16: Kali partition.*

<img width="533" height="155" alt="Fig_17 Kali mounted files" src="https://github.com/user-attachments/assets/ef208b80-df35-4a03-9e5b-89c3055db5c9" />


*Figure 17: Kali mounted files.*

---

## Part E: MBR, GPT and Boot Process Explanation

### E1. Explain the Boot Process

- **Power-On:** The computer receives power and starts the boot process.
- **POST (Power-On Self-Test):** Hardware components such as RAM, CPU, and storage are checked.
- **BIOS/UEFI Firmware:** The firmware initializes hardware and reads boot configuration settings.
- **CMOS/Firmware Settings:** The system retrieves the configured boot order and other startup parameters.
- **EFI Boot Partition / GPT:** The firmware locates the EFI boot files required to start Windows.
- **Boot Manager:** Windows Boot Manager loads the operating system.
- **Kernel Loading:** The Windows kernel and necessary drivers are loaded into memory.
- **Operating System Control:** Windows takes control of the system and displays the login screen.

**Boot mode for my system**

It uses UEFI boot mode. This is shown by the presence of an EFI System Partition (FAT32) observed in the partition table. The UEFI firmware reads the EFI boot files from this partition and starts the Windows Boot Manager, which loads the Windows kernel and transfers control to the operating system.

---

## Part F: File System Identification and Interpretation

**To Identify Two File Systems**

**Table 5**

| Volume | File System |
|---|---|
| C: Volume | NTFS |
| EFI System Partition | FAT32 |

**The reason why file systems matter in digital forensics**

A file system is important in digital forensics because it organizes and stores data on a storage device. It contains evidence such as file names, timestamps, and deleted file records, helping investigators recover data, reconstruct user activity, and establish timelines.

---

## Part G: Final Practical Assignment

### Partition 1 (EFI System Partition)

**Table 6**

| Item | Value |
|---|---|
| Partition | Partition EFI |
| Size | 100 MB |
| Sector size | 512 bytes |
| File system | NTFS |

**Calculations**

```text
100 MB x 1024 x 1024 = 104,857,600 bytes

Number of Sectors = 104,857,600 / 512
                  = 204,800 sectors

Partition Size (GB) = 204,800 x 512 / 1024 / 1024 / 1024
                    = 0.0977 GB
                    = 0.10 GB
```

### Partition 2 (MSR Partition)

**Table 7**

| Item | Value |
|---|---|
| Partition | Partition 2 (MSR) |
| Size | 16 MB |
| Sector size | 512 bytes |
| File system | N/A (Microsoft Reserved Partition) |

**Calculations**

```text
16 MB x 1024 x 1024 = 16,777,216 bytes

Number of Sectors = 16,777,216 / 512
                  = 32,768 sectors

Partition Size (GB) = 32,768 x 512 / 1024 / 1024 / 1024
                    = 0.0156 GB
                    = 0.016 GB
```

### Partition Size Comparison

| Partition | Calculated Size | OS-Reported Size |
|---|---:|---:|
| Partition 1 (EFI) | 0.10 GB | 100 MB |
| Partition 2 (MSR) | 0.016 GB | 16 MB |

There is no significant difference between the calculated size and the operating system reported size. Minor differences occur because Windows displays partition sizes in MB, while the calculation converts the values to GB and may involve rounding.

<img width="647" height="149" alt="Fig_18 Partition" src="https://github.com/user-attachments/assets/80187a1c-7d1d-4765-8472-7969a51e7903" />


*Figure 18: Partition.*

<img width="593" height="104" alt="Fig_19 Volume" src="https://github.com/user-attachments/assets/cc83e7d9-0a13-4499-a6a9-51fcf9ca7740" />


*Figure 19: Volume.*

---

## Conclusion

This report demonstrates how digital forensic evidence can be identified and analyzed from system processes, file systems, partitions, and storage devices. Using Disk Management, PowerShell, DiskPart, Task Manager, and Kali Linux command-line tools, partition information is verified, file systems are identified, and process activity is examined. The exercises reinforce fundamental digital forensics techniques used in evidence collection, verification, and analysis.

---




