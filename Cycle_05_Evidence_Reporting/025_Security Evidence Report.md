# 🛡️ Security Evidence Report

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?style=flat-square&logo=powershell&logoColor=white) ![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square) ![Category](https://img.shields.io/badge/Category-Evidence%20Reporting-informational?style=flat-square) ![Security](https://img.shields.io/badge/Security-Evidence%20Collection-orange?style=flat-square) ![Project](https://img.shields.io/badge/Project-Security%20Evidence%20Report-purple?style=flat-square)

---

## 📌 Executive Summary

**Core Technical Focus:**
Collected the computer name, process count, service count, and Downloads file count into a single `PSCustomObject`, displayed it as a report, and exported it to CSV.

**Defensive Impact:**
A structured, exportable evidence record lets findings be shared, compared against later collections, and reviewed by someone else instead of living only in a console window.

---

## 🎯 Objective & Challenge

*Collect baseline evidence from a flagged workstation, organize it into a structured report, and provide an evidence-based assessment.*

---

## 🔐 Cybersecurity Context

A workstation was flagged for review after a user reported unusual system behavior. The analyst needed to record:

- Which computer was investigated
- How many processes were running
- How many services were present
- How many files were in Downloads
- Whether any collection warnings occurred

Evidence that is structured and exported can be attached to a ticket, compared with a known-good baseline, or handed off to another analyst.

---

## 🛠️ Implementation & Code

### Create Evidence Report

```powershell
$CompReport = [PSCustomObject]@{
    ComputerName    = $env:COMPUTERNAME
    ProcessCount    = (Get-Process).Count
    ServiceCount    = (Get-Service).Count
    DownloadedFiles = (Get-ChildItem "$env:USERPROFILE\Downloads").Count
}
```

<img width="739" height="116" alt="image" src="https://github.com/user-attachments/assets/bbf433c2-5570-4be7-b92f-653eb21eb542" />

### Example Usage

```powershell
$CompReport | Format-List
```

Example Output:

```text
PS C:\> $CompReport | Format-List

ComputerName    : [REDACTED]
ProcessCount    : 326
ServiceCount    : 320
DownloadedFiles : 181
```

---

### Additional Examples

#### Export Evidence Report

```powershell
$CompReport |
    Export-Csv "$env:USERPROFILE\Downloads\Lesson25.csv" -NoTypeInformation
```

<img width="731" height="51" alt="image" src="https://github.com/user-attachments/assets/56ec28df-dc67-4f82-bf87-95d499cbe983" />

#### Collection Warnings

During earlier evidence collection, these services generated access-related warnings when queried:

```text
IsolationSession
WaaSMedicSvc
```

These warnings did not prevent evidence collection from completing.

---

### Analyst Assessment

| Evidence | Result |
|--------|--------|
| Computer name | [REDACTED] |
| Process count | 326 |
| Service count | 320 |
| Downloaded files | 181 |

No suspicious activity was identified during baseline evidence collection. Service counts were within expected ranges, and the Downloads inventory size was consistent with routine workstation use.

Additional investigation would only be warranted if:

- New indicators of compromise (IOCs) are identified.
- A suspicious file or process is reported.
- Additional evidence sources suggest abnormal behavior.

---

## 🆕 New Command Introduced

### Export-Csv

This project introduced the `Export-Csv` cmdlet to address **preserving investigation results outside the PowerShell session**.

Example:

```powershell
$CompReport |
    Export-Csv "$env:USERPROFILE\Downloads\Lesson25.csv" -NoTypeInformation
```

### Why It Was Useful

Console output disappears when the window closes. Exporting the report to CSV created a record that can be opened in Excel, attached to a ticket, or compared with a later collection.

### Comparison

| Approach | Benefit | Limitation |
|-----------|-----------|-----------|
| `Format-List` / console output | Easy to read immediately | Lost when the session ends |
| `Export-Csv` | Persistent, shareable, comparable | Flattens nested objects |

---

## ✅ Key Takeaways

- **Structured Evidence:** Used `PSCustomObject` to combine several evidence sources into one record.
- **Readable Reporting:** Used `Format-List` to present the report clearly.
- **Evidence Preservation:** Used `Export-Csv` to save the findings for later review.
- **Honest Assessment:** Documented collection warnings and stated clearly what would justify escalation.

---

## 🧠 Lessons Learned

This challenge combined the Evidence Reporting cycle into one workflow: collect, structure, display, and export. Compared with running separate commands and reading the console, the report object made the evidence consistent and the export made it durable. Recording the access warnings mattered too, because a good report says what could not be collected as well as what was.

### New Concepts Integration

`Export-Csv` turned the investigation from something that only existed on screen into an artifact. That is the difference between running commands and producing evidence, and it sets up the reusable audit function built in Cycle 6.
