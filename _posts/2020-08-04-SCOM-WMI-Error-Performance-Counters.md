---
title: "Operations Manager failed to Retrieve Raw Performance Data"
comments: true
tags: [SCOM,Software]
excerpt: "Operations Manager issued a critical alert for two performance monitors. To fix this scenario you need to rebuild the performance counters."
---
I logged into work this morning with two critical alerts from System Center
Operations Manager from one of our Domain Controllers:

1. `The LSASS process has exceeded the processor utilization threshold`
2. `The total number of ATQ threads in use has exceeded one or more thresholds over multiple samples.`

The reporting server was not maxing CPU usage, RAM, or system resources.
The alert descriptions provided some additional insights. Respectively, they were:

`Failure to retrieve the raw performance data for the lsass process via WMI: The error returned was: 'Generic failure ' (0x80041001)`
and
`Failure to retrieve the raw performance data for NTDS via WMI: The error returned was: 'Object required' (0x1A8)`  

## The Fix
The solution was to [rebuild Performance Counter Library values](https://support.microsoft.com/en-us/help/300956/how-to-manually-rebuild-performance-counter-library-values).
This is achieved by navigating to
`C:\Windows\System32`  
Run `lodtcr /R`  

**Error Message:** Error: Unable to rebuild performance counter setting from system backup store, error code is 2.  
{: .notice--info}

If you encounter the above error, switch directories to `C:\Windows\syswow64`   
Then run `lodtcr /R`  

For the above tip, special thanks to this [question post](https://social.technet.microsoft.com/Forums/ie/en-US/9b01e1a6-d872-4f28-9280-f35d6ca02a9f/lodctr-r-error-code-2?forum=w7itprogeneral).
