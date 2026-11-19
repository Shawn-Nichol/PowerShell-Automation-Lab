# 🛡️ Process Inventory Audit

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?style=flat-square&logo=powershell&logoColor=white) ![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square) ![Category](https://img.shields.io/badge/Category-Large%20Dataset%20Analysis-informational?style=flat-square) ![Security](https://img.shields.io/badge/Security-Process%20Analysis-orange?style=flat-square) ![Project](https://img.shields.io/badge/Project-Process%20Inventory%20Audit-purple?style=flat-square)

---

## 📌 Executive Summary

**Core Technical Focus:**
Stored the full process list in a variable, then counted, searched, and sorted that collection to build a process inventory without re-querying the system for every question.

**Defensive Impact:**
A process inventory gives analysts a baseline of what is running on a flagged workstation, which makes unusual processes easier to spot.

---

## 🎯 Objective & Challenge

*Build a process inventory for a flagged workstation, check for PowerShell activity, and identify the heaviest processes for review.*

---

## 🔐 Cybersecurity Context

A workstation was flagged for unusual activity and the security team requested a process inventory to establish a baseline of running processes.

- How many processes are running?
- Is PowerShell running? (It is a common attacker tool, so its presence is worth noting.)
- Which processes are consuming the most resources?
- What should be documented for follow-up?

---

## 🛠️ Implementation & Code

### Collect Process Inventory

```powershell
$process = Get-Process

Write-Host "Total Running Processes:" $process.Count
```

Example Output:

```text
PS C:\> $process = Get-Process
>>
>> Write-Host "Total Running Processes:" $process.Count
Total Running Processes: 362
```

---

### Search for PowerShell Activity

```powershell
Get-Process |
    Where-Object ProcessName -like "*power*"
```

```powershell
$process |
    Where-Object ProcessName -like "*powershell*"
```

<img width="502" height="116" alt="image" src="https://github.com/user-attachments/assets/de7637ac-84bb-4006-a80b-e7200b96f5d6" />

Result:

```text
No matching processes returned
```

---

### Memory Analysis

```powershell
$process |
    Sort-Object WorkingSet -Descending |
    Select-Object -First 5 ProcessName, Id, WorkingSet
```

```text
PS C:\> $process |
>>     Sort-Object WorkingSet -Descending |
>>     Select-Object -First 5 ProcessName, Id, WorkingSet

ProcessName           Id WorkingSet
-----------           -- ----------
[REDACTED]         20044  871481344
msedgewebview2     23476  569020416
Memory Compression  3672  545234944
chrome              3096  314191872
chrome             17072  304488448
```

---

### Analyst Assessment

- The system was running between 362 and 366 processes across collection runs.
- No process matching `*powershell*` or `*power*` was returned. Note that PowerShell 7 runs as `pwsh.exe`, so a complete check should also search for `pwsh`.
- The largest memory consumers were the endpoint security agent, Edge WebView2, Memory Compression, and Chrome, all of which are expected on a corporate workstation.
- A large process inventory is normal when browsers, system services, and security tools run at the same time. Further investigation would be needed to determine whether any process is unusual for the environment.

---

## 🆕 New Command Introduced

### Collections and `.Count`

This project introduced storing command output in a variable as a collection to address **asking several questions of the same data without re-running `Get-Process` each time**.

Example:

```powershell
$process = Get-Process
$process.Count
```

### Why It Was Useful

Capturing the inventory once means every count, search, and sort works on the same snapshot of the system. That keeps the results consistent with each other and makes the investigation faster.

### Comparison

| Approach | Benefit | Limitation |
|-----------|-----------|-----------|
| Re-running `Get-Process` per question | Always current | Each answer comes from a different snapshot |
| Stored collection (`$process`) | One consistent snapshot, faster | Goes stale if the system changes |

---

## ✅ Key Takeaways

- **Inventory Baseline:** Used `.Count` on a stored collection to establish the number of running processes.
- **Threat-Relevant Search:** Used wildcard filtering to look for PowerShell activity.
- **Resource Ranking:** Sorted the collection to identify the heaviest processes.
- **Consistent Evidence:** Analyzed one snapshot instead of mixing results from multiple queries.

---

## 🧠 Lessons Learned

This challenge applied the Large Dataset Analysis cycle to a realistic request: produce a process inventory for a flagged machine. Storing the data once and then filtering, counting, and sorting it made the workflow faster and the findings consistent. It also showed that a negative result needs checking: searching for "powershell" can miss PowerShell 7, which runs as `pwsh`.

### New Concepts Integration

Working with a stored collection is what makes larger datasets manageable. Instead of re-querying the system, the analyst captures the evidence once and then asks it as many questions as needed, a pattern that carries into file system and log analysis later in the lab.
