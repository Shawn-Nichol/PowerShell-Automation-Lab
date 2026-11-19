# 🛡️ Exporting Data to CSV in PowerShell

[![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-5391FE?logo=powershell&logoColor=white)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Category](https://img.shields.io/badge/Category-Evidence%20Reporting-blue)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Security](https://img.shields.io/badge/Security-Evidence%20Collection-red)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Concept](https://img.shields.io/badge/Concept-CSV%20Export-orange)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)

## 📌 Executive Summary

This lesson introduced the `Export-Csv` cmdlet and demonstrated how PowerShell can preserve investigation results in a structured format. Rather than relying on screenshots or console output, CSV files allow analysts to save, share, sort, filter, and revisit findings during future investigations.

---

## 🎯 Learning Objective

Learn how to export PowerShell data to CSV files and understand how exported evidence supports investigation, reporting, and documentation workflows.

---

## 🔐 Cybersecurity Application

Security analysts frequently export evidence collected during investigations. CSV reports can be used to preserve process inventories, file inventories, service audits, user reviews, and other investigative findings.

Exported data can then be:

- Reviewed in Excel
- Shared with other analysts
- Stored as investigation evidence
- Compared with future investigations
- Included in incident response documentation

---

## ✅ Key Takeaways

- `Export-Csv` saves PowerShell output to a CSV file.
- CSV files preserve investigation results for future review.
- `Select-Object` can be used to export only relevant information.
- The `-NoTypeInformation` parameter removes unnecessary metadata.
- CSV files can be opened in Excel and other reporting tools.
- Structured evidence is more useful than screenshots during investigations.

---

## 💻 PowerShell Commands

### Export a Process Inventory

```powershell
Get-Process |
    Select-Object ProcessName, Id |
    Export-Csv "$env:USERPROFILE\Desktop\ProcessInventory.csv" -NoTypeInformation
```

```text
PS C:\> Get-Process |
>>     Select-Object ProcessName, Id |
>>     Export-Csv "$env:USERPROFILE\Documents\ProcessInventory.csv" -NoTypeInformation
PS C:\> Get-ChildItem "$env:USERPROFILE\Documents\" |
>>     Where-Object Name -Like "Process*"

    Directory: C:\Documents

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           9/18/2026  3:35 PM           7943 ProcessInventory.csv
```


### Export a Downloads Inventory

---

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" |
    Select-Object Name, Length, LastWriteTime |
    Export-Csv "$env:USERPROFILE\Desktop\DownloadsInventory.csv" -NoTypeInformation
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" |
>>     Select-Object Name, Length, LastWriteTime |
>>     Export-Csv "$env:USERPROFILE\Documents\DownloadsInventory.csv" -NoTypeInformation
PS C:\> Get-ChildItem "$env:USERPROFILE\Documents\" |
>>     Where-Object Name -Like "Downloads*"

    Directory: C:\Documents

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           9/18/2026  3:58 PM          11985 DownloadsInventory.csv
```

---

### Export Executable Files

```powershell
Get-ChildItem "$env:USERPROFILE\Downloads" |
    Where-Object Extension -eq ".exe" |
    Select-Object Name, Length, LastWriteTime |
    Export-Csv "$env:USERPROFILE\Desktop\Executables.csv" -NoTypeInformation
```

```text
PS C:\> Get-ChildItem "$env:USERPROFILE\Downloads" |
>>     Where-Object Extension -eq ".exe" |
>>     Select-Object Name, Length, LastWriteTime |
>>     Export-Csv "$env:USERPROFILE\Documents\Executables.csv" -NoTypeInformation
PS C:\> Get-ChildItem "$env:USERPROFILE\Documents\" |
>>     Where-Object Name -Like "Exe*"

    Directory: C:\Documents

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           9/18/2026  4:01 PM           1769 Executables.csv
```


---

## 🧠 What I Learned

- Learned how to export PowerShell data into CSV files.
- Practiced selecting only relevant properties before exporting.
- Learned how to remove PowerShell type information using `-NoTypeInformation`.
- Gained an understanding of how evidence can be preserved for future review.
- Learned why cybersecurity analysts often export investigation results rather than relying on screenshots.
