# 🛡️ Discovering PowerShell Commands
![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue?logo=powershell)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-brightgreen)
![Category](https://img.shields.io/badge/Category-Command%20Discovery-blue)
![Security](https://img.shields.io/badge/Security-Cybersecurity-red)
![Concept](https://img.shields.io/badge/Concept-Help%20System-orange)



## 📌 Executive Summary

This lab introduced PowerShell command discovery and the built-in help system. I learned how to locate relevant commands using wildcards, read documentation using Get-Help, and view running processes. These skills form the foundation for future cybersecurity investigations.


## 🎯 Learning Objective

Learn how to discover PowerShell commands and understand their usage through PowerShell's built-in help system.


## 🚀 Key Highlights

- Used `Get-Command` to discover available commands.
- Used wildcards to find related cmdlets.
- Used `Get-Help` to access examples and documentation.
- Viewed running processes with `Get-Process`.
- Connected process enumeration to cybersecurity investigations.


## 🔐 Cybersecurity Application

Cybersecurity analysts regularly encounter unfamiliar systems. The ability to discover commands and read built-in documentation allows analysts to investigate processes, services, logs, and system settings efficiently without relying on memorization.


## 💻 PowerShell Commands

```powershell
Get-Command
```
<img width="872" height="581" alt="image" src="https://github.com/user-attachments/assets/6989b295-dea5-4ebf-be2c-fe8f5226e22a" />


```powershell
Get-Command *process*
```
<img width="930" height="386" alt="image" src="https://github.com/user-attachments/assets/1b3c5ade-0358-4047-bebc-b64bf7811d91" />


```powershell
Get-Command *service*
```
<img width="932" height="765" alt="image" src="https://github.com/user-attachments/assets/e4d00e6e-6acb-48f0-a113-6c4063bdf1fa" />


```powershell
Get-Help Get-Service -Examples
```
<img width="941" height="499" alt="image" src="https://github.com/user-attachments/assets/dba2cb8a-808d-42a5-a288-2cb82a2f9f9e" />



```powershell
Get-Process |
Select-Object -First 10
```
```text
PS H:\> Get-Process |
>> Select-Object -First 10

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    502      55    44220       8176      50.30  17668   1 advanced_ip_scanner
    156      10     2376       4420              8676   0 AggregatorHost
    350      41    11304      21016              1704   1 AOTListenerService
    366      20    12812      23364       2.34  14712   1 AppActions
    562      27    64716      22552       4.84  13424   1 ApplicationFrameHost
     74       8     1424       1636       0.22  15596   1 AppUIMonitor
   1206      85    62112      48796              4512   0 [REDACTED]
    476      27     7196      12528              4784   0 [REDACTED]
   1703      58    38312      49784              5280   0 [REDACTED]
    303      24    27488      31324       1.53   1232   1 chrome
```


## ✅ Key Takeaways

- PowerShell commands can be discovered instead of memorized.
- Wildcards make searching for commands easy.
- Get-Help provides built-in documentation and examples.
- Process visibility is a core cybersecurity skill.
