# 🛡️ Suspicious Files Review

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?style=flat-square&logo=powershell&logoColor=white) ![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square) ![Category](https://img.shields.io/badge/Category-File%20System%20Investigation-informational?style=flat-square) ![Security](https://img.shields.io/badge/Security-File%20Triage-orange?style=flat-square) ![Project](https://img.shields.io/badge/Project-Suspicious%20Files%20Review-purple?style=flat-square)

---

## 📌 Executive Summary

**Core Technical Focus:**
Collected every file in the Downloads directory (including hidden ones) with `Get-ChildItem -Force`, then used sorting and filtering to review file counts, large files, executables, hidden files, and file age.

**Defensive Impact:**
Downloads is one of the most common places for malicious files to land. A structured review lets an analyst quickly separate routine software from files that need deeper analysis.

---

## 🎯 Objective & Challenge

*Review a user's Downloads directory after reports of unusual system behavior and determine whether any files appear suspicious.*

---

## 🔐 Cybersecurity Context

A user reported unusual system behavior after downloading multiple files. As the investigating analyst, I needed to answer:

- How many files are present?
- Which files are the largest?
- Which executables need review?
- Are there hidden files, and what are they?
- What conclusions can be drawn?

Malware often arrives as an executable or archive in Downloads, and hidden attributes are sometimes used to keep files out of sight. A methodical review of size, type, visibility, and age helps decide where to focus.

---

## 🛠️ Implementation & Code

### Collect Files

```powershell
$file = Get-ChildItem "$env:USERPROFILE\Downloads" -Force
```

<img width="571" height="53" alt="image" src="https://github.com/user-attachments/assets/6b493ea9-cb9e-48e7-806a-498978500510" />

### Count Files

```powershell
$file.Count
```

<img width="288" height="35" alt="image" src="https://github.com/user-attachments/assets/9a15c8af-067d-49c6-9f27-395b7e85e31c" />

### Identify Largest Files

```powershell
$file |
Sort-Object Length -Descending |
Select-Object -First 5 Name, Length
```

```text
PS C:\> $file |
>>     Sort-Object Length -Descending |
>>     Select-Object -First 5 Name, Length

Name                                                              Length
----                                                              ------
Windows.iso                                                   4893900800
[REDACTED].exe                                                 850371936
[REDACTED].exe                                                 501667168
[REDACTED] (1).exe                                             407143776
Microsoft.WebView2.FixedVersionRuntime.143.0.3650.66.x64.cab   275910565
```

---

### Additional Examples

#### Count Executables

```powershell
($file | Where-Object Extension -eq ".exe").Count
```

<img width="485" height="44" alt="image" src="https://github.com/user-attachments/assets/2690b7da-1abc-474e-81dc-5a4e65b2a3ce" />

#### Review Executables

```powershell
$file |
Where-Object Extension -eq ".exe"
```

```text
PS C:\> $file |
>>     Where-Object Extension -eq ".exe"

    Directory: C:\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           4/27/2026 10:02 AM       18543040 audacity-win-3.7.7-64bit.exe
-a---           6/18/2026  2:36 PM       20400664 audacity-win-3.7.8-64bit.exe
-a---           7/22/2026  2:30 PM        9221652 BCUninstaller_6.2.0_setup.exe
-a---           2/26/2026 11:11 AM      407143776 [REDACTED] (1).exe
-a---           12/4/2025  2:20 PM      501667168 [REDACTED].exe
-a---           12/4/2025  2:22 PM      850371936 [REDACTED].exe
-a---           12/6/2025  1:52 PM       33267856 driver_booster_setup.exe
-a---           5/12/2026  1:47 PM       19463448 MediaCreationTool_22H2.exe
-a---           12/6/2025  1:40 AM        1694136 MicrosoftEdgeWebview2Setup (1).exe
...
```

#### Find Newest Executable

```powershell
$file |
Where-Object Extension -eq ".exe" |
Sort-Object LastWriteTime -Descending |
Select-Object -First 1 Name, LastWriteTime
```

```text
PS C:\> $file |
>>     Where-Object Extension -eq ".exe"

    Directory: C:\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           4/27/2026 10:02 AM       18543040 audacity-win-3.7.7-64bit.exe
-a---           6/18/2026  2:36 PM       20400664 audacity-win-3.7.8-64bit.exe
-a---           7/22/2026  2:30 PM        9221652 BCUninstaller_6.2.0_setup.exe
-a---           2/26/2026 11:11 AM      407143776 [REDACTED] (1).exe
-a---           12/4/2025  2:20 PM      501667168 [REDACTED].exe
-a---           12/4/2025  2:22 PM      850371936 [REDACTED].exe
-a---           12/6/2025  1:52 PM       33267856 driver_booster_setup.exe
-a---           5/12/2026  1:47 PM       19463448 MediaCreationTool_22H2.exe
-a---           12/6/2025  1:40 AM        1694136 MicrosoftEdgeWebview2Setup (1).exe
...
```

#### Count Hidden Files

```powershell
($file | Where-Object Attributes -like "*Hidden*").Count
```

<img width="585" height="40" alt="image" src="https://github.com/user-attachments/assets/689d30a4-30ec-46d5-b1b8-3bfc1d4926fa" />

#### Review Hidden Files

```powershell
$file |
Where-Object Attributes -like "*Hidden*"
```

```text
PS C:\> $file |
>>     Where-Object Attributes -like "*Hidden*"

    Directory: C:\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a-h-           9/25/2025 10:48 AM            162 ~$ble labels WOA .docx
-a-h-           7/17/2024  9:49 PM            162 ~$itical Thinking Touchstone 4.docx
-a-h-          10/28/2025  1:30 PM            162 ~$ndows11_Minimum_Requirement_Check_Commands [REDACTED].docx
-a-h-          11/10/2025 11:28 AM            162 ~$rformance Through Partnership - F2026 Coaching Guide.docx
-a-h-           9/24/2025  5:03 PM          21860 ~WRL0005.tmp
-a-h-           7/15/2024 11:30 PM          34360 ~WRL0284.tmp
-a-h-           11/4/2025  3:46 PM          88663 ~WRL1962.tmp
-a-h-           7/20/2024 12:48 AM          33773 ~WRL3265.tmp
-a-hs           12/7/2025 10:12 PM            282 desktop.ini
```

#### Identify Largest Hidden Files

```powershell
$file |
Where-Object Attributes -like "*Hidden*" |
Sort-Object Length -Descending |
Select-Object -First 5 Name, Length
```

<img width="466" height="209" alt="image" src="https://github.com/user-attachments/assets/844dd747-bdb6-4b63-abe1-aebccbea7c46" />

#### Identify Oldest Files

```powershell
$file |
Sort-Object LastWriteTime |
Select-Object -First 5 Name, LastWriteTime
```

<img width="520" height="197" alt="image" src="https://github.com/user-attachments/assets/bd7236a0-203b-481c-b2b0-30b177424960" />

#### Largest Non-Executable Files

```powershell
$file |
Where-Object Extension -ne ".exe" |
Sort-Object Length -Descending |
Select-Object -First 5 Name, Length
```

```text
PS C:\> $file |
>>     Where-Object Extension -ne ".exe" |
>>     Sort-Object Length -Descending |
>>     Select-Object -First 5 Name, Length

Name                                                              Length
----                                                              ------
Windows.iso                                                   4893900800
Microsoft.WebView2.FixedVersionRuntime.143.0.3650.66.x64.cab   275910565
SysinternalsSuite.zip                                          193515765
[REDACTED].zip                                                 144989707
[REDACTED] (1).zip                                             144989707
```

---

### Investigation Findings

| Metric | Result |
|--------|--------|
| Total files | 175 |
| Executable files | 27 |
| Hidden files | 9 |

#### Largest Files

| File | Size |
|--------|--------|
| Windows.iso | 4.89 GB |
| [REDACTED].exe | 850 MB |
| [REDACTED].exe | 501 MB |
| [REDACTED] (1).exe | 407 MB |
| Microsoft.WebView2.FixedVersionRuntime.143.0.3650.66.x64.cab | 275 MB |

The largest file, `Windows.iso` (about 4.89 GB), appears to be legitimate Windows installation media.

#### Largest Executables

| File | Size |
|--------|--------|
| [REDACTED].exe | 850 MB |
| [REDACTED].exe | 501 MB |
| [REDACTED] (1).exe | 407 MB |
| [REDACTED].exe | 84 MB |
| voxpro5_2_0_1_setup.exe | 49 MB |

The executables are associated with remote access clients, VoxPro, and other administrative and productivity tools. No obviously suspicious executable names were identified.

#### Newest Executable

`BCUninstaller_6.2.0_setup.exe`, modified July 22, 2026. The filename is consistent with Bulk Crap Uninstaller, a legitimate software removal utility commonly used by technical users and administrators.

#### Hidden Files

| File | Size |
|--------|--------|
| ~WRL1962.tmp | 88 KB |
| ~WRL0284.tmp | 34 KB |
| ~WRL3265.tmp | 33 KB |
| ~WRL0005.tmp | 21 KB |
| desktop.ini | 282 B |

The hidden files are temporary application files, Microsoft Office lock files, and standard Windows configuration files. No hidden executables or unusually large hidden files were identified.

#### Oldest Files

| File | Modified |
|--------|--------|
| ~WRL0284.tmp | July 15, 2024 |
| ~$itical Thinking Touchstone 4.docx | July 17, 2024 |
| ~WRL3265.tmp | July 20, 2024 |
| ~WRL0005.tmp | September 24, 2025 |
| ~$ble labels WOA.docx | September 25, 2025 |

Several hidden temporary files date back to 2024, showing that the Downloads directory holds remnants of older activity and has never been fully cleaned.

#### Largest Non-Executable Files

| File | Size |
|--------|--------|
| Windows.iso | 4.89 GB |
| Microsoft.WebView2.FixedVersionRuntime.143.0.3650.66.x64.cab | 275 MB |
| SysinternalsSuite.zip | 193 MB |
| [REDACTED].zip | 145 MB |
| [REDACTED] (1).zip | 145 MB |

`SysinternalsSuite.zip` is noteworthy because Sysinternals tools are used by administrators and responders, but also by attackers. In this context it is consistent with the user's technical role.

### Analyst Assessment

The software found (remote access clients, Audacity, Microsoft WebView2, Sysinternals Suite, VoxPro) is consistent with a workstation used for technical support, administration, and broadcast operations.

Based on file size analysis, the executable inventory, the hidden file review, and file age analysis, no immediately suspicious artifacts were identified. Further investigation would be warranted if:

- A specific IOC was provided.
- A suspicious filename was identified.
- A timeline-based investigation was required.
- Process or network activity suggested a compromised file.

**Next steps to strengthen this review:** check file hashes with `Get-FileHash`, verify publisher signatures with `Get-AuthenticodeSignature`, and read the `Zone.Identifier` stream to confirm where each executable was downloaded from.

---

## 🆕 New Command Introduced

### Get-ChildItem -Force

This project used the `-Force` parameter of `Get-ChildItem` to address **hidden files being left out of a normal directory listing**.

Example:

```powershell
$file = Get-ChildItem "$env:USERPROFILE\Downloads" -Force
```

### Why It Was Useful

Without `-Force`, hidden and system files do not appear in the results, which is exactly where something trying to avoid notice would sit. Adding it made the hidden file review possible.

### Comparison

| Approach | Benefit | Limitation |
|-----------|-----------|-----------|
| `Get-ChildItem` | Clean, readable listing | Silently skips hidden and system files |
| `Get-ChildItem -Force` | Complete listing, including hidden files | More noise to filter through |

---

## ✅ Key Takeaways

- **Complete Collection:** Used `-Force` so hidden files were included in the evidence.
- **Prioritized Review:** Used sorting and filtering to focus on large, executable, hidden, and old files.
- **Context-Driven Analysis:** Judged each finding against the user's role rather than treating every tool as suspicious.
- **Documented Assessment:** Produced an evidence-based conclusion with clear criteria for escalation.

---

## 🧠 Lessons Learned

This challenge combined the file discovery, attribute analysis, and filtering techniques from the File System Investigation cycle into one review. Breaking the directory down by size, type, visibility, and age turned 175 files into a short list of things worth explaining, and each one had a legitimate explanation. The review also showed the limits of name-based analysis: hashes, signatures, and download origin would make the conclusion much stronger.

### New Concepts Integration

The `-Force` parameter is a small change with a big impact. A file listing that hides hidden files gives an incomplete picture, and in an investigation an incomplete picture can lead to a wrong conclusion. Collecting everything first and filtering afterwards is the safer pattern.
