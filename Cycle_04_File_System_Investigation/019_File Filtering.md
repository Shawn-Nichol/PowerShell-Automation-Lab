# 🛡️ File Filtering in PowerShell

[![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-5391FE?logo=powershell&logoColor=white)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Category](https://img.shields.io/badge/Category-Filesystem%20Investigation-blue)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Security](https://img.shields.io/badge/Security-File%20Triage-red)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Concept](https://img.shields.io/badge/Concept-File%20Filtering-orange)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
## 📌 Executive Summary

This lesson focused on filtering files to identify the most relevant artifacts during a filesystem investigation. By sorting files by size and modification date, and filtering executables, PowerShell can quickly reduce large file inventories into manageable sets for review.


## 🎯 Learning Objective

Learn how to filter, sort, and prioritize files using PowerShell based on attributes such as size, file type, and modification date.


## 🔐 Cybersecurity Application

Security analysts rarely review every file individually. Instead, they focus on files that stand out based on characteristics such as:

- Large file size
- Recent modification date
- Executable extensions
- Hidden attributes
- Unusual locations

These techniques help analysts identify suspicious artifacts more efficiently during investigations.


## ✅ Key Takeaways

- `Where-Object` can filter files based on properties.
- `Sort-Object` can prioritize results by size or date.
- The `Length` property contains file size information.
- The `LastWriteTime` property contains modification timestamps.
- Large files and recent files are often useful investigation starting points.
- File filtering reduces noise during security investigations.


## 💻 PowerShell Commands

### Find the Largest Files

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" |
    Sort-Object Length -Descending |
    Select-Object -First 5 Name, Length
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" |
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

### Find the Most Recently Modified Files

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" |
    Sort-Object LastWriteTime -Descending |
    Select-Object -First 5 Name, LastWriteTime
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" |
>>     Sort-Object LastWriteTime -Descending |
>>     Select-Object -First 5 Name, LastWriteTime

Name                                                            LastWriteTime
----                                                            -------------
Best+of+Fal.Con+2026.ics                                        9/14/2026 4:13:11 PM
[REDACTED].csv                                                  9/11/2026 11:44:30 AM
notam-request---with-instructions---obstacle-light-outage.pdf   9/9/2026 10:52:25 AM
[REDACTED].pdf                                                  9/9/2026 10:28:59 AM
CHQR HD-09-08-2026-08-00.mp3                                    9/8/2026 12:25:58 PM
```

---

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" -Recurse |
    Where-Object Extension -eq ".exe" |
    Sort-Object Length -Descending |
    Select-Object -First 5 Name, Length
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" -Recurse |
>>     Where-Object Extension -eq ".exe" |
>>     Sort-Object Length -Descending |
>>     Select-Object -First 5 Name, Length

Name                          Length
----                          ------
[REDACTED].exe             850371936
[REDACTED].exe             501667168
[REDACTED] (1).exe         407143776
[REDACTED].exe              83986912
voxpro5_2_0_1_setup.exe     48779896
```

---

### Find Files Larger Than 100 MB

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" |
    Where-Object Length -gt 100MB
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" |
>>     Where-Object Length -gt 100MB

    Directory: C:\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           2/26/2026 11:11 AM      407143776 [REDACTED] (1).exe
-a---           12/4/2025  2:20 PM      501667168 [REDACTED].exe
-a---           12/4/2025  2:22 PM      850371936 [REDACTED].exe
-a---          10/22/2025 11:31 AM      124694528 clockify-setup.msi
-a---           12/6/2025 12:27 PM      275910565 Microsoft.WebView2.FixedVersionRuntime.143.0.3650.66.x64.cab
-a---           7/14/2026  1:31 PM      193515765 SysinternalsSuite.zip
-a---           7/23/2026  2:12 PM      144989707 [REDACTED] (1).zip
-a---           7/21/2026  2:18 PM      144989707 [REDACTED].zip
-a---           5/12/2026  3:15 PM     4893900800 Windows.iso
```


---

## 🧠 What I Learned

- Learned how to prioritize files using sorting and filtering techniques.
- Practiced identifying large files and recently modified files.
- Used the `Length` property to review file sizes.
- Used the `LastWriteTime` property to review modification dates.
- Applied filtering techniques to executable files.
- Gained an understanding of how analysts reduce large data sets into focused investigation targets.
