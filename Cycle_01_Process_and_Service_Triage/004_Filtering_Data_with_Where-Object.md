# 📘 Lesson 004: Filtering Data with Where-Object

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?style=flat-square&logo=powershell&logoColor=white) ![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square) ![Category](https://img.shields.io/badge/Category-Process%20%26%20Service%20Triage-informational?style=flat-square) ![Type](https://img.shields.io/badge/Type-Lesson-lightgrey?style=flat-square)

---

## 💡 Concept

*`Where-Object` filters the objects coming down the pipeline and keeps only the ones whose property matches a condition, such as `Status -eq 'Running'`.*

---

## 🔐 Why It Matters in Security

- Analysts rarely need every result. Filtering cuts hundreds of processes or services down to the few that answer the question.
- It quickly isolates suspicious conditions: stopped security services, unresponsive processes, or a specific process name.
- Filtering is the first step in almost every hunt, for example searching for `powershell` processes, which attackers commonly abuse.

---

## 🛠️ Code & Output

### Find Running Services

```powershell
Get-Service |
Where-Object Status -eq 'Running'
```

```text
PS H:\> Get-Service |
>> Where-Object Status -eq 'Running'

Status   Name               DisplayName
------   ----               -----------
Running  Appinfo            Application Information
Running  AppXSvc            AppX Deployment Service (AppXSVC)
Running  AudioEndpointBu... Windows Audio Endpoint Builder
Running  Audiosrv           Windows Audio
Running  [REDACTED]         [REDACTED]
Running  BFE                Base Filtering Engine
Running  BITS               Background Intelligent Transfer Ser...
Running  BluetoothUserSe... Bluetooth User Support Service_11b2b4c
Running  [REDACTED]         [REDACTED]
Running  BrokerInfrastru... Background Tasks Infrastructure Ser...
Running  BTAGService        Bluetooth Audio Gateway Service
Running  BthAvctpSvc        AVCTP service
Running  bthserv            Bluetooth Support Service
```

### Find Stopped Services

```powershell
Get-Service |
Where-Object Status -eq 'Stopped'
```

<img width="621" height="311" alt="image" src="https://github.com/user-attachments/assets/317e4993-d670-4ddd-b285-eac8e3e382c9" />

### Search for PowerShell Processes

```powershell
Get-Process |
Where-Object ProcessName -like '*powershell*'
```

<img width="688" height="130" alt="image" src="https://github.com/user-attachments/assets/e512db5f-4e93-4fdb-92cb-8560ba6bdcfc" />

### Find Unresponsive Processes

```powershell
Get-Process |
Where-Object Responding -eq $false
```

<img width="685" height="137" alt="image" src="https://github.com/user-attachments/assets/5e4f7f7a-70c4-48f5-bba2-264e78ec44ed" />

---

## ✅ Key Takeaways

- **Filter, don't scroll:** `Where-Object` returns only the objects that match a property condition.
- **Comparison operators:** `-eq`, `-ne`, and `-like` (with `*` wildcards) define what to keep.
- **Gotcha:** PowerShell 7 runs as `pwsh`, so `-like '*powershell*'` misses it. Search for both names when hunting.
