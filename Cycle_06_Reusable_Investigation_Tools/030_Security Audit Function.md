# 🛡️ Security Audit Function

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?style=flat-square&logo=powershell&logoColor=white) ![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square) ![Category](https://img.shields.io/badge/Category-Reusable%20Investigation%20Tools-informational?style=flat-square) ![Security](https://img.shields.io/badge/Security-Security%20Auditing-orange?style=flat-square) ![Project](https://img.shields.io/badge/Project-Security%20Audit%20Function-purple?style=flat-square)

---

## 📌 Executive Summary

**Core Technical Focus:**
Built a reusable `Get-SecurityAudit` function that takes the analyst's name as a parameter and returns a `PSCustomObject` with the computer name, service count, process count, and Downloads file count, ready to export.

**Defensive Impact:**
Turns a set of manual baseline commands into one consistent tool, so every workstation audit collects the same evidence in the same format.

---

## 🎯 Objective & Challenge

*Create a reusable function that collects workstation baseline information and returns it in a structured, exportable format.*

---

## 🔐 Cybersecurity Context

Security analysts frequently collect baseline information during investigations and audits. Running the same commands by hand on every machine is slow and inconsistent.

Reusable functions help:

- Standardize evidence collection
- Reduce manual effort and mistakes
- Record who performed the audit
- Feed reporting workflows
- Lay the foundation for broader security automation

---

## 🛠️ Implementation & Code

### Security Audit Function

```powershell
function Get-SecurityAudit {

    Param(
        $Name
    )

    [PSCustomObject]@{
        AnalystName  = $Name
        ComputerName = $env:COMPUTERNAME
        Services     = (Get-Service).Count
        Processes    = (Get-Process).Count
        Downloads    = (Get-ChildItem "$env:USERPROFILE\Downloads").Count
        Assessment   = "Baseline audit Completed"
    }
}
```

<img width="661" height="244" alt="image" src="https://github.com/user-attachments/assets/2d4be951-9594-416f-aa11-afd58bb7ff96" />

### Example Usage

```powershell
$ComputerInfo = Get-SecurityAudit -Name "Shawn"
$ComputerInfo
```

<img width="598" height="19" alt="image" src="https://github.com/user-attachments/assets/6363e683-3726-4d9a-ab76-ab6b01c9031e" />

Example Output:

```text
AnalystName  : Shawn
ComputerName : [REDACTED]
Services     : 323
Processes    : 363
Downloads    : 200
Assessment   : Baseline audit Completed
```

<img width="336" height="106" alt="image" src="https://github.com/user-attachments/assets/7a0a64bc-51a2-49fb-aa8c-81084915200b" />

---

### Additional Examples

#### Export Results

```powershell
$ComputerInfo |
    Export-Csv "$Home\Downloads\Summary.csv" -NoTypeInformation
```

<img width="600" height="32" alt="image" src="https://github.com/user-attachments/assets/c32be348-7526-496b-8a40-d6f047a60e9b" />

The audit report was exported to `Summary.csv`, preserving the results for sharing and later comparison.

---

### Analyst Assessment

The function collected:

- Analyst name
- Computer name
- Service count
- Running process count
- Downloads folder inventory count

This provides a basic workstation baseline that can be compared against future audits. The function can be extended to include event logs, network connections, local users, and installed software.

---

## 🆕 New Command Introduced

### function with Param()

This project combined `function` and `Param()` to address **repeating the same multi-command audit by hand on every workstation**.

Example:

```powershell
function Get-SecurityAudit {
    Param(
        $Name
    )
    # audit logic
}
```

### Why It Was Useful

Wrapping the audit in a function means it runs with one command and always collects the same fields. The `$Name` parameter records who performed the audit, which matters when evidence is reviewed later.

### Comparison

| Approach | Benefit | Limitation |
|-----------|-----------|-----------|
| Individual commands | Flexible | Inconsistent between runs and analysts |
| Parameterized function | Repeatable, consistent output | Needs updating as requirements grow |

---

## ✅ Key Takeaways

- **Reusable Tooling:** Built a function that performs a full baseline audit with one command.
- **Analyst Attribution:** Used a parameter to record who performed the audit.
- **Structured Output:** Returned results as a `PSCustomObject` so they can be filtered, formatted, or exported.
- **Evidence Preservation:** Exported the audit to CSV for reporting and comparison.

---

## 🧠 Lessons Learned

This challenge pulled together everything from the first six cycles: commands, variables, objects, reporting, and functions. Instead of running several commands manually, one function now gathers a consistent baseline and returns it in a form that can be exported or compared. That consistency is what makes results trustworthy across different machines and different analysts.

### New Concepts Integration

Functions with parameters are the step from scripting to tooling. Once the audit logic lives in `Get-SecurityAudit`, it can be reused, extended, and eventually run against multiple machines, which is where the remote investigation cycles later in this lab are heading.
