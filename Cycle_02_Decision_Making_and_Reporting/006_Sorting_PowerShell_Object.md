# 🛡️ Sorting PowerShell Objects

[![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-5391FE?logo=powershell&logoColor=white)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Category](https://img.shields.io/badge/Category-Pipeline%20%26%20Sorting-blue)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Security](https://img.shields.io/badge/Security-Triage%20%26%20Investigation-red)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)
[![Concept](https://img.shields.io/badge/Concept-Sort--Object-orange)](https://github.com/Shawn-Nichol/PowerShell-Automation-Lab-)


## 📌 Executive Summary

This lesson introduced object sorting with `Sort-Object`. By sorting process data, I learned how to quickly identify resource-intensive applications and create focused investigation reports.


## 🎯 Learning Objective

Learn how to sort PowerShell objects using object properties and prioritize the most relevant results.


## 🔐 Cybersecurity Application

Sorting allows analysts to rapidly identify unusual behavior such as high CPU consumption, excessive memory usage, or abnormal system activity during investigations.


## ✅ Key Takeaways

- `Sort-Object` sorts PowerShell objects.
- `-Descending` reverses the sort order.
- Sorting can prioritize investigation targets.
- `Select-Object -First` limits output to the most relevant entries.
- Sorting improves report readability.


## 💻 PowerShell Commands

```powershell
Get-Process |
Sort-Object CPU -Descending
```
```text
PS C:\> Get-Process |
>> Sort-Object CPU -Descending

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2800      65   297736     119544   5,702.69   1756   1 dwm
    474      26   234676     166852   5,153.42  10056   0 [REDACTED]
    535      95   817680     612244   5,050.64   9420   0 [REDACTED]
   7134       0       52        136   3,828.72      4   0 System
   1450     115   214432     140916   2,358.98  26704   1 [REDACTED]
    440      42   555804     354184   1,965.64  23640   1 chrome
    608      88   955428     795636   1,810.31   3540   1 msedgewebview2
   1890      19    27296      32280   1,610.52   4420   0 svchost
    524      46   539064      64768   1,317.98  20476   1 msedgewebview2
   1611      66   116260      37008   1,303.41  16344   1 ms-teams
   1075      21     8692      11420   1,136.41   1116   0 services
   2877     111   474292     421360   1,077.63  26780   1 chrome
    944      32    70736      74304   1,011.48   4628   0 [REDACTED]
   1305      96   151388     158524     956.31   7036   1 Todo
    457      73   871260     861944     882.67  27924   1 chrome
   2579      77   410228     290820     775.52  22012   1 chrome
    206      14     8992      15772     761.41  12932   0 WmiPrvSE
    332      30   235924     167812     747.83   1764   1 msedgewebview2
    477      23    54060      49360     661.17   4864   0 svchost
      0       0     4496     793016     638.14   3700   0 Memory Compression
```


```powershell
Get-Process |
Sort-Object CPU -Descending |
Select-Object ProcessName,
              Id,
              CPU
```

```text
PS C:\> Get-Process |
>> Sort-Object CPU -Descending |
>> Select-Object ProcessName,
>>              Id,
>>              CPU

ProcessName                       Id         CPU
-----------                       --         ---
dwm                             1756  5708.21875
[REDACTED]                     10056 5162.484375
[REDACTED]                      9420  5051.21875
System                             4  3831.15625
[REDACTED]                     26704 2364.078125
chrome                         23640  1965.65625
msedgewebview2                  3540 1810.359375
svchost                         4420    1611.625
msedgewebview2                 20476 1317.984375
ms-teams                       16344 1303.453125
services                        1116 1137.671875
chrome                         26780      1078.5
[REDACTED]                      4628 1012.734375
Todo                            7036  956.765625
chrome                         27924  883.671875
chrome                         22012  776.015625
WmiPrvSE                       12932  761.953125
msedgewebview2                  1764   747.84375
```


```powershell
Get-Process |
Sort-Object CPU -Descending |
Select-Object -First 5 ProcessName,
                        Id,
                        CPU
```
```text
PS C:\> Get-Process |
>> Sort-Object CPU -Descending |
>> Select-Object -First 5 ProcessName,
>>                        Id,
>>                        CPU

ProcessName      Id        CPU
-----------      --        ---
dwm            1756 5715.65625
[REDACTED]    10056       5164
[REDACTED]     9420     5051.5
System            4  3835.5625
[REDACTED]    26704 2370.90625
```


## 🧠 What I Learned

- How to sort PowerShell object data.
- How to find high-resource processes quickly.
- How to combine sorting and selection.
- How sorting supports cybersecurity investigations.
