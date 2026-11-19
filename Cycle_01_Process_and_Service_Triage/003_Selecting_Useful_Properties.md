# 🛡️ Selecting Useful Properties

[![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-5391FE?logo=powershell&logoColor=white)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Category](https://img.shields.io/badge/Category-Pipeline%20%26%20Properties-blue)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Security](https://img.shields.io/badge/Security-Incident%20Response-red)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Concept](https://img.shields.io/badge/Concept-Select--Object-orange)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)



## 📌 Executive Summary

This session focused on using `Select-Object` to display only the properties relevant to an investigation or troubleshooting task. Instead of accepting PowerShell's default view, I learned how to customize output to highlight specific information.


## 🎯 Learning Objective

Learn how to use `Select-Object` to extract useful information from PowerShell objects.


## 🔐 Cybersecurity Application

Analysts often need only a small subset of available data. Selecting specific properties makes process investigations, service audits, and incident response activities more efficient.


## ✅ Key Takeaways

- `Select-Object` controls which properties are displayed.
- Objects contain more information than the default view shows.
- Choosing relevant properties improves readability.
- Process and service data can be tailored for investigations.


## 💻 PowerShell Commands

```powershell
Get-Process |
Select-Object ProcessName,
              Id
```
```text
PS C:\WINDOWS\system32> Get-Process |
>> Select-Object ProcessName,
>> Id -First 10

ProcessName             Id
-----------             --
AggregatorHost        8676
AOTListenerService    1704
AppActions           14712
ApplicationFrameHost 13424
AppUIMonitor         15596
audiodg              23892
[REDACTED]            4512
[REDACTED]            4784
[REDACTED]            5280
chrome                2032
```


```powershell
Get-Process |
Select-Object ProcessName,
              Id,
              Responding
```
```text
PS C:\WINDOWS\system32> Get-Process |
>> Select-Object ProcessName,
>> Id,
>> Responding -First 10

ProcessName             Id Responding
-----------             -- ----------
AggregatorHost        8676       True
AOTListenerService    1704       True
AppActions           14712       True
ApplicationFrameHost 13424       True
AppUIMonitor         15596       True
[REDACTED]            4512       True
backgroundTaskHost   23036       True
[REDACTED]            4784       True
[REDACTED]            5280       True
chrome                2032       True
```

```powershell
Get-Service |
Select-Object Name,
              Status
```
<img width="411" height="285" alt="image" src="https://github.com/user-attachments/assets/70856c32-1996-49c7-b9d0-431876702857" />


```powershell
Get-Process |
Select-Object ProcessName,
              Id,
              WorkingSet64 |
Sort-Object WorkingSet64 -Descending
```
```text
PS C:\WINDOWS\system32> Get-Process |
>> Select-Object ProcessName,
>> Id,
>> WorkingSet64 |
>> Sort-Object Workingset64 -Descending |
>> Select-Object -First 10

ProcessName            Id WorkingSet64
-----------            -- ------------
msedgewebview2      20696   1007202304
msedgewebview2       6500    894038016
[REDACTED]           9336    715669504
chrome               3608    555532288
Memory Compression   3632    539697152
Grammarly.Desktop   26592    366534656
chrome              16808    362377216
explorer            13784    302583808
chrome              31236    237596672
chrome              22988    231211008
```


## 🧠 What I Learned

- How to select specific properties from PowerShell objects.
- The difference between default output and customized output.
- How to display process and service information efficiently.
- How memory usage can help identify resource-intensive processes.

---
