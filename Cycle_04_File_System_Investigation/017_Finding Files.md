# 🛡️ Finding Files in PowerShell

[![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-5391FE?logo=powershell&logoColor=white)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Category](https://img.shields.io/badge/Category-Filesystem%20Investigation-blue)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Security](https://img.shields.io/badge/Security-File%20Hunting-red)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Concept](https://img.shields.io/badge/Concept-File%20Discovery-orange)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)

## 📌 Executive Summary

This lesson focused on locating files within the Windows filesystem using PowerShell. By searching for specific file types and reviewing file inventories, it becomes possible to quickly identify artifacts that may be relevant during a cybersecurity investigation.


## 🎯 Learning Objective

Learn how to search for files using `Get-ChildItem`, the `-Filter` parameter, and recursive searches through subdirectories.

## 🔐 Cybersecurity Application

Security analysts frequently search for specific file types during investigations. Common examples include executables, PowerShell scripts, archives, and configuration files. Quickly locating these files helps establish timelines, identify suspicious artifacts, and collect evidence.


## ✅ Key Takeaways

- `Get-ChildItem` can be used to search for files.
- The `-Filter` parameter narrows search results.
- Recursive searches include subfolders.
- File inventories help establish investigation baselines.
- File hunting is an essential incident response and threat-hunting skill.
- Reviewing file names and dates can reveal unusual activity.

---

## 💻 PowerShell Commands

### Search for MP3 Files

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.mp3"
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.mp3"

    Directory: C:\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           11/3/2025 10:51 AM       28768886 CFGQ PGM-11-01-2025-19-00.mp3
-a---          11/26/2025  5:02 AM       28781633 CFGQ PGM-11-26-2025-04-00.mp3
-a---           11/3/2025 10:48 AM       28791246 CFGQ-11-01-2025-19-00.mp3
-a---           11/3/2025 10:46 AM       28779961 CFGQ-11-01-2025-20-00.mp3
-a---          11/26/2025  5:04 AM       28781633 CFGQ-11-26-2025-04-00.mp3
-a---          12/16/2025 11:18 AM       28784768 CFGQ-12-15-2025-21-00.mp3
-a---          12/16/2025 11:18 AM       28796053 CFGQ-12-15-2025-22-00.mp3
-a---          12/16/2025 11:18 AM       28788112 CFGQ-12-15-2025-23-00.mp3
-a---            9/8/2026 12:25 PM       28795808 CHQR HD-09-08-2026-08-00.mp3
-a---            9/8/2026 11:25 AM       28786440 CHQR PGM-09-08-2026-08-00.mp3
-a---            9/8/2026 10:39 AM       14398090 CHQR-09-08-2026-07-00.mp3
-a---            9/8/2026 10:39 AM       14400494 CHQR-09-08-2026-08-00.mp3
-a---           4/27/2026 10:01 AM       28792918 CKRY PGM-04-24-2026-13-00.mp3
-a---           4/27/2026 10:01 AM       28792918 CKRY PGM-04-24-2026-14-00 (1).mp3
-a---           4/27/2026 10:00 AM       28792918 CKRY PGM-04-24-2026-14-00.mp3
-a---           4/27/2026 10:03 AM       28616957 CKRY-04-24-2026-13-00.mp3
```


### Search for ZIP Files

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.zip"
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.zip"

    Directory: C:\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---            8/5/2026 10:39 AM        3246652 currentLog_20260805.zip
-a---            8/5/2026 10:48 AM       10789347 DetectabilityHistoryL50067 (1).zip
-a---            8/5/2026 10:41 AM       10789081 DetectabilityHistoryL50067.zip
-a---          10/17/2025  3:32 PM          67574 Logs.zip
-a---            8/4/2026  4:14 PM          24958 Psw.zip
-a---          12/12/2025  3:12 PM        4501577 SennheiserAssetCollection-2025-12-12 22_12.zip
-a---           7/14/2026  1:31 PM      193515765 SysinternalsSuite.zip
-a---           7/23/2026  2:12 PM      144989707 [REDACTED] (1).zip
-a---           7/21/2026  2:18 PM      144989707 [REDACTED].zip
```


### Search for CSV Files

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.csv"
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.csv"

    Directory: C:\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           8/11/2026  4:16 PM          54892 [REDACTED].csv
-a---            9/1/2026  3:34 PM           6988 [REDACTED].csv
-a---           8/12/2026  3:16 PM           6184 [REDACTED].csv
-a---           9/11/2026 11:44 AM          99395 [REDACTED].csv
-a---          10/29/2025  4:14 PM           3022 Wide Orbit Checklist(Sheet1).csv
```


### Search for Executable Files Recursively

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.exe" -Recurse
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" -Filter "*.exe" -Recurse

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


---

## 🧠 Wha* I Learned

- Learned how to search for files by extension.
- Practiced using the `-Filter` parameter to narrow results.
- Used recursive searches to include subfolders.
- Reviewed file inventories to identify patterns and anomalies.
- Gained experience applying file hunting techniques to a cybersecurity investigation scenario.

---

## 🔍 Investigation Notes

### MP3 File Review

```text
16 MP3 files found
```

Observations:

- Most files followed a*similar naming convention.
- The f*les appeared to originate from an audio logging system.
- No unusual file names were identified.

### ZIP File Review

```text
9 ZIP files found
```

Observations:

- The newest ZIP file was dated August 8, 2026.
- No suspicious archive names were observed during the review.

### CSV File Review

```text
5 CSV files found
```

Examples included:

```text
Telnet Password
WideOrbit Checklist
```

Observations:

- Some filenames suggested configuration or operational information.
- Files referencing passwords would warrant additional review in a real investigation.

### Executable File Review

```text
27 EXE files found
```
Observations:

- No file names appeared suspicious.
- No executable files older than 2026 were identified.
- The Downloads folder may have been cleaned or reorganized at some point.

---

## 🧠 Analyst Reflection

The purpose of file hunting is not simply to count files but to understand what is normal for a system.

During this investigation I focused on:

- File types present in the Downloads directory.
- Naming conventions.
- File age.
- Potentially sensitive artifacts.

No obvious anomalies were identified, but the exercise demonstrated how PowerShell can quickly locate evidence that may require further investigation.

