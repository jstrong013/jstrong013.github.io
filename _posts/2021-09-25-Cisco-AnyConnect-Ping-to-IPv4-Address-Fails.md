---
title: "Cisco AnyConnect Ping to IPv4 Address Fails"
comments: true
tags: [Software,Network]
excerpt: "Cisco AnyConnect version 4.x fails to do a nslookup or returns the response on
a ping to an IP address "could not find host" despite having an active connection established."
---
Hello friends - I had a new critical case submission that came up.
A ticket came across my queue where the User was connected to
Cisco AnyConnect VPN but was unable to reach any company DFS/mapped drive despite
having an active connection. Our configuration was Cisco AnyConnect version
4.10.00093, ASA 5516 on version 9.14(2)15, and ASDM version 7.14(1) with a split-tunnel.
I quickly responded to the User with a Quick Assist screen share. I opened up PowerShell prompt
to dig deeper:
* Verified VPN connection established (Trust, but verify as we have split-tunnel)
* Pinging the file share server hostname yielded no results
* Pinging the IPv4 address was successful

Interesting. At this rate, I new it was DNS (It's always DNS). But no one else at the company was having
issues. So, I flushed the DNS Cache Resolver and rebooted the workstation. At this stage,
the User was able to retrieve their files from the file share.

Sporadically, over time, other Users were reporting this same scenario.  The workaround continued
to fix the situation. At this rate, I was quite confused.

* DNS server was online and operational with no warnings/errors
* ````Route Print```` on the workstation showed the routes injected
* ASA Configuration verified (pretty basic)
* The DART bundle reported routes being injected into the virtual adapter
* Most Users only had the issue once

At this rate, I knew I needed a packet capture.

## Solving the DNS Issue
**Warning:** Modifying the Windows Registry has inherit risks. As always and before most changes, make
a backup if you deem relevant in your environment.
{: .notice--warning}

To resolve, I created the following registry key (no reboot necessary):

    HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters
    DisableParallelAandAAAA (DWord) to Value 1
Of course, I have to include the PowerShell way:
````Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters" -Name DisableParallelAandAAAA -Value 1 -Type DWord````

You may also have to create an additional registry key entry (I did not have to do this)

    HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows NT\DNSClient
    DisableSmartNameResolution (DWord) to Value 1
The almighty PowerShell way:
````Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows NT\DNSClient" -Name DisableSmartNameResolution -Value 1 -Type DWord````  

While untested in my lab, I believe you may achieve the same result with a Group Policy:

      Computer Configuration > Administrative Templates > Network > DNS Client
      Set Turn off smart multi-homed name resolution to Enabled

### Background and resource links
Upon a little bit of research, it appears a new feature was added in Windows 8 (our environment was running Windows 10).
The feature, SmartNameResolution, makes DNS queries to all network adapters. The first one
to respond is determined to be the source of truth. In reality, with VPN, it is a
DNS leak. To prevent this "optimization," then you need to disable the behavior.

Cisco provides this modification in their release notes:
* https://www.cisco.com/c/en/us/td/docs/security/vpn_client/anyconnect/anyconnect410/release/notes/release-notes-anyconnect-4-10.html
* https://www.cisco.com/c/en/us/td/docs/security/asdm/7_14/release/notes/rn714.html

Well, I hope this helps ya out. Have a wonderful day!
