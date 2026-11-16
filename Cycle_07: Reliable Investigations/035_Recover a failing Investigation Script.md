# 🛡️ Recover a Failing Investigation Script

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?style=flat-square&logo=powershell&logoColor=white) ![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square) ![Category](https://img.shields.io/badge/Category-Reliable%20Investigations-informational?style=flat-square) ![Security](https://img.shields.io/badge/Security-Error%20Handling-orange?style=flat-square) ![Project](https://img.shields.io/badge/Project-Recover%20a%20Failing%20Investigation%20Script-purple?style=flat-square)
---

## 📌 Executive Summary

**Core Technical Focus:**  
This script automates process investigation by combining structured error recovery, persistent file logging, and precise timestamps into a single reusable function.

**Defensive Impact:**  
By handling missing targets gracefully and logging every action, the script ensures analysts maintain a continuous audit trail and reliable visibility during workstation triage.

---

## 🎯 Objective & Challenge

*Investigate target processes gracefully, record activity to a persistent log, timestamp investigation actions, and maintain consistent execution regardless of whether the target process exists.*

---

## 🔐 Cybersecurity Context

During workstation investigations, evidence frequently disappears before analysis begins.

- Malware may have already terminated.
- User applications may have been closed.
- Short-lived processes may no longer be running.
- Incorrect process names may be supplied during triage.

A useful investigation tool should continue operating and documenting findings even when the requested process cannot be located.

Equally important is maintaining documentation that allows the investigation to be reviewed later. An investigation should leave behind enough information to determine:

- What was investigated
- When the investigation occurred
- Whether evidence was located
- When the investigation completed

This project focused on building those capabilities directly into the investigation workflow rather than relying solely on console output.

---

## 🛠️ Implementation & Code

### Process Investigation Function

```powershell
function Investigate-Process {

    Param(
        $ProcessName
    )

    $FileLocation = "$Home\Downloads\InvestigationLog.txt"

    "Investigation Started - $(Get-Date)" |
         Tee-Object $FileLocation

    try {

        Get-Process -Name $ProcessName -ErrorAction Stop |
            Tee-Object $FileLocation -Append

    }
    catch {

        "$ProcessName doesn't exist" |
            Tee-Object $FileLocation -Append

    }
    finally {

         "Investigation Complete - $(Get-Date)" |
            Tee-Object $FileLocation -Append

    }
}
```

### Example Usage

```powershell
Investigate-Process -ProcessName explorer
```

Example Output:

```text
Investigation Started - 11/16/2026 13:22:41

explorer process information

Investigation Complete - 11/16/2026 13:22:41
```

SCREENSHOT_HERE

---

### Example Usage

```powershell
Investigate-Process -ProcessName NoProcess
```

Example Output:

```text
Investigation Started - 11/16/2026 13:19:56

NoProcess doesn't exist

Investigation Complete - 11/16/2026 13:19:56
```

SCREENSHOT_HERE

---

### Example Investigation Log

```text
Investigation Started - 11/16/2026 13:19:56
NoProcess doesn't exist
Investigation Complete - 11/16/2026 13:19:56
```

SCREENSHOT_HERE

---

## 🆕 New Command Introduced

### Tee-Object

This project introduced the `Tee-Object` cmdlet to address the challenge of displaying investigation results while simultaneously maintaining a persistent log file.

Example:

```powershell
"Investigation Started - $(Get-Date)" |
    Tee-Object $FileLocation
```

### Why It Was Useful

Previous logging exercises primarily relied on:

```powershell
Out-File
```

which records information to a file but does not display the same information to the analyst.

`Tee-Object` allowed investigation activity to be:

- Displayed immediately in the PowerShell console
- Written to the investigation log file
- Kept synchronized without duplicating commands

### Comparison

| Approach | Benefit | Limitation |
|-----------|-----------|-----------|
| Out-File | Simple file logging | No console output |
| Tee-Object | Simultaneous console output and logging | Slightly more complex syntax |

---

## ✅ Key Takeaways

- **Graceful Recovery:** Used `Try` and `Catch` blocks to investigate processes without producing unhandled errors.
- **Reliable Execution:** Used a `Finally` block to guarantee completion messages and logging activity.
- **Audit Trail Creation:** Generated a persistent investigation record containing timestamps and results.
- **Analyst Visibility:** Used `Tee-Object` to provide real-time feedback while preserving the same information in a log file.

---

## 🧠 Lessons Learned

This project demonstrated how combining logging, error handling, timestamps, and reusable functions creates a more reliable and repeatable investigation workflow. Instead of stopping when a process could not be found, the function now handles expected failures gracefully, records the outcome, and maintains a consistent execution path from start to finish. The result is a more practical investigation utility that provides immediate analyst feedback while preserving a permanent record of the investigation.

### New Concepts Integration

The introduction of `Tee-Object` solved a core logging limitation by eliminating the need for separate commands to display data in the console and write it to disk. By passing the pipeline stream through `Tee-Object`, investigation output is captured simultaneously on screen and in a persistent log file. This keeps feedback loops instantaneous for the analyst while ensuring a reliable, synchronized audit record is always preserved.
