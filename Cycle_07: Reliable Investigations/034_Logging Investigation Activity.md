# 🛡️ Logging Investigation Activity

![PowerShell 7+](https://img.shields.io/badge/PowerShell-7%2B-239120?style=for-the-badge&logo=powershell&logoColor=white)
![Difficulty: Beginner](https://img.shields.io/badge/Difficulty-Beginner-brightgreen?style=for-the-badge)
![Category: Reliable Investigations](https://img.shields.io/badge/Category-Reliable%20Investigations-007EC6?style=for-the-badge)
![Security: Evidence Collection](https://img.shields.io/badge/Security-Evidence%20Collection-informational?style=for-the-badge)
![Concept: Investigation Logging](https://img.shields.io/badge/Concept-Investigation%20Logging-orange?style=for-the-badge)
---

## 📌 Executive Summary

This lesson introduced investigation logging and demonstrated how PowerShell can create and maintain an audit trail of investigation activity. Logging allows analysts to record actions, findings, timestamps, and workflow progress so investigations can be reviewed and verified later.

Rather than relying on memory or console history, a log file provides a permanent record of what occurred, when it occurred, and what actions were taken.

---

## 🎯 Learning Objective

Learn how to create log files, append information to existing logs, record timestamps, and build reusable logging functions using PowerShell.

---

## 🔐 Cybersecurity Application

Logging is an important component of cybersecurity investigations because it provides accountability and traceability.

Common uses include:

- Incident response documentation
- Threat hunting activity tracking
- Security audit records
- Evidence collection logs
- Investigation timelines
- Troubleshooting and reporting

A well-maintained log file can help answer questions such as:

```text
When did the investigation start?
Who performed the investigation?
What actions were taken?
When were findings discovered?
```

---

## ✅ Key Takeaways

- `Out-File` writes information to a file.
- `-Append` adds information without overwriting existing data.
- Timestamps help build an investigation timeline.
- Logging provides an audit trail of investigation activity.
- Functions can automate log creation.
- Investigation logs support reporting and evidence collection.

---

## 💻 PowerShell Commands

### Create Initial Log Entry

```powershell
"Investigation Started - $(Get-Date)" |
    Out-File "$Home\Downloads\InvestigationLog.txt"
```

<img width="525" height="37" alt="image" src="https://github.com/user-attachments/assets/c308ba19-fb31-49c7-b46c-70033d9367dd" />

<img width="407" height="67" alt="image" src="https://github.com/user-attachments/assets/a19b1eb9-da1e-4a36-a0dc-3f67b50753c9" />

---

### Append Additional Investigation Activity

```powershell
"Process Review Complete - $(Get-Date)" |
    Out-File "$Home\Downloads\InvestigationLog.txt" -Append
```

<img width="512" height="33" alt="image" src="https://github.com/user-attachments/assets/18cb60ce-90fa-4bb9-8fe4-846d68acdb8c" />
<img width="438" height="54" alt="image" src="https://github.com/user-attachments/assets/01cb5fc3-5c2e-4953-868b-3094053a6cf9" />


---

### Investigation Logging Function

```powershell
function Write-Investigation {

    Param(
        $Message
    )

    "$Message - $(Get-Date)" |
        Out-File "$Home\Downloads\InvestigationLog.txt" -Append

}
```

<img width="554" height="123" alt="image" src="https://github.com/user-attachments/assets/b3bef4e0-2489-4498-97e6-f30fa319c75b" />

---

### Write a Log Entry

```powershell
Write-Investigation -Message "Starting system check"
```

<img width="525" height="19" alt="image" src="https://github.com/user-attachments/assets/6ba03aec-5b09-44df-b31f-b09dd9ddb1b4" />

<img width="431" height="80" alt="image" src="https://github.com/user-attachments/assets/249e9714-0f8e-49c7-b963-ad5ef916e505" />


---

## 🧠 What I Learned

- Learned how to create investigation log files.
- Used `Out-File` to write information to disk.
- Used `-Append` to preserve existing log entries.
- Added timestamps to investigation activity.
- Built a reusable logging function.
- Learned how logging supports incident response and evidence collection.

---

## 🔍 Investigation Notes

### Investigation Log Contents

The log file contained the following entries:

```text
Investigation Started - 11/13/2026 14:57:50
Process Review Complete - 11/13/2026 14:59:07
Starting system check - 11/13/2026 15:01:49
```

These entries demonstrate how a log file can capture both activity and timing information.

---

### Timestamp Verification

Timestamps were successfully written to the log file using:

```powershell
$(Get-Date)
```

This provides an accurate record of when each action occurred during the investigation.

---

### Logging Function Review

The logging function automated the process of:

- Adding messages
- Recording timestamps
- Appending to the existing log file

This reduced repetitive commands and created a standardized logging workflow.

---

### Investigation Timeline

The resulting log file created a simple timeline:

| Time | Activity |
|--------|--------|
| 14:57:50 | Investigation Started |
| 14:59:07 | Process Review Complete |
| 15:01:49 | Starting System Check |

This demonstrates how logging can be used to reconstruct investigation activity after the fact.

---

## 🧠 Analyst Reflection

One of the most important habits an analyst can develop is documenting investigation activity as it occurs.

Without logging:

```text
Evidence may be collected but there is no record of what actions were performed.
```

With logging:

```text
Every action can be documented, timestamped, and reviewed later.
```

A simple log file provides valuable context during investigations and creates an audit trail that can be used for reporting, troubleshooting, and evidence preservation.

By combining timestamps with reusable logging functions, it becomes possible to build investigation tools that automatically document their own activity and generate a complete record of the investigation process.
