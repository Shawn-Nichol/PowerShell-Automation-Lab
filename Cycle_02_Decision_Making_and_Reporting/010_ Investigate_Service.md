# 🛡️ Suspicious Service Investigation

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?style=flat-square&logo=powershell&logoColor=white) ![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square) ![Category](https://img.shields.io/badge/Category-Decision%20Making%20%26%20Reporting-informational?style=flat-square) ![Security](https://img.shields.io/badge/Security-SOC%20Investigation-orange?style=flat-square) ![Project](https://img.shields.io/badge/Project-Suspicious%20Service%20Investigation-purple?style=flat-square)

---

## 📌 Executive Summary

**Core Technical Focus:**
Built an interactive script that takes a service name from the analyst, checks whether it exists with `Get-Service`, and reports its status and start type in a structured object.

**Defensive Impact:**
Gives analysts a quick, repeatable way to verify a service named in an alert before deciding whether deeper investigation is needed.

---

## 🎯 Objective & Challenge

*Determine whether a service named in a security alert exists, whether it is running, and what to collect next.*

---

## 🔐 Cybersecurity Context

A security alert indicated that a Windows service may be involved in suspicious activity. Attackers often create or modify services to gain persistence, so an analyst needs to answer quickly:

- Does the service exist?
- Is it currently running?
- What is its startup configuration?
- What should be collected next to decide whether it is legitimate?

---

## 🛠️ Implementation & Code

### Service Investigation Script

```powershell
# Get user input (Store as a string)
$serviceName = Read-Host "Enter service"

# Query the service object
$serviceObj = Get-Service -Name $serviceName -ErrorAction SilentlyContinue

# Determine existence and current state
if($serviceObj) {

    Write-Host "$serviceName exists and status is: $($serviceObj.Status)`n"

    # Investigation report
    [PSCustomObject]@{
        "Service Name" = $serviceObj.Name
        "DisplayName" = $serviceObj.DisplayName
        "Status" = $serviceObj.Status
        "Start Type" = $serviceObj.StartType
    } | Format-Table -AutoSize

}
else {

    Write-Host "Service '$serviceName' was not found on this system." -ForegroundColor Red

}
```

### Example Usage

```text
Enter service: spooler
```

Example Output:

<img width="412" height="123" alt="image" src="https://github.com/user-attachments/assets/fd8927ea-3a20-487d-ba2c-0b500bde2980" />

---

### Analyst Assessment

A service being present and running does not automatically mean it is legitimate.

Additional evidence that should be collected during a security investigation includes:

- Service executable path
- Service owner / account
- Digital signature information
- Startup type
- Associated processes
- Network activity
- Business purpose

Confirming that a service exists is only the first step of an investigation.

---

## 🆕 New Command Introduced

### Read-Host

This project introduced the `Read-Host` cmdlet to address **making a script reusable for any service name instead of hard-coding one**.

Example:

```powershell
$serviceName = Read-Host "Enter service"
```

### Why It Was Useful

Without `Read-Host`, the script would need editing every time a different service was investigated. Prompting the analyst for input turned a one-off command into a small, reusable investigation tool.

### Comparison

| Approach | Benefit | Limitation |
|-----------|-----------|-----------|
| Hard-coded service name | Simple | Must edit the script for every investigation |
| `Read-Host` input | Reusable for any service | Interactive only; not suited to automation |

---

## ✅ Key Takeaways

- **Interactive Investigation:** Used `Read-Host` to build a script that works for any service.
- **Conditional Logic:** Used `if`/`else` to handle both found and missing services cleanly.
- **Structured Reporting:** Presented results as a `PSCustomObject` for consistent, readable output.
- **Investigation Mindset:** Identified what evidence to collect next instead of stopping at "it exists".

---

## 🧠 Lessons Learned

This challenge combined variables, user input, conditional logic, and service enumeration into one workflow. Instead of running several separate commands, the script validates the service, reports its state, and handles the "not found" case without throwing errors. The main lesson was that investigations are about gathering and validating evidence before reaching conclusions.

### New Concepts Integration

`Read-Host` made the script reusable. The same logic now works for any service an alert names, which is the first step toward the parameterized functions built later in Cycle 6.
