---
title: "Should I rename the Windows Administrator Account?"
comments: true
tags: [Opinion, PowerShell]
excerpt: "Should you rename the default Windows Administrator Account? My answer, no."
---
My friend locked himself out of his laptop. While I was able to get him into the laptop 
by resetting the local Windows Administrator password, he asked me an interesting question 
that inspired this post.  

His question, "Should I rename the local Windows Administrator account?"  

You do not have to read this entire post to get my answer. My answer is **no**. 
Instead, I would suggest to disable the local Administrator account 
or implement a long, secure, and rotating password solution such as Microsoft's [Local Administrator Password Solution (LAPS)](https://learn.microsoft.com/en-us/windows-server/identity/laps/laps-overview). On top of this, if you are in 
an Enterprise environment I would encourage a solution that alerts IT staff on too many 
bad password attempts on the Administrator account (even if it is disabled).  

## Okay, Why Not?  
Microsoft has a setting to [rename the local administrator](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/accounts-rename-administrator-account) account. 

On that settings page, it even says: 

> Because the administrator account exists on all Windows 10 for desktop editions (Home, Pro, Enterprise, and Education), renaming the account makes it slightly more difficult for attackers to guess this user name and password combination.  

This, in essence, is a true statement. The problem arises that it is trivial to 
detect the administrator account(s). All accounts and groups posses security identifiers (SIDs).  

The SID for the Administrator account is S-1-5-<value>-**500**.  

To prove the above, open a PowerShell prompt on your Windows workstation. 

```powershell
Get-LocalUser -Name Administrator | Select-Object Name,SID,Description | Format-List
```

The returned value will look something like this: 

```
Name        : Administrator
SID         : S-1-5-21-2467591965-3378450126-2336327866-500
Description : Built-in account for administering the computer/domain
``` 

Now, feel free to rename the Administrator account. To get this account, I only 
have to search by the SID: 

```powershell
Get-LocalUser | Where-Object -Property SID -like "*500" | Select-Object Name,SID,Description | Format-List
``` 

Despite the rename of your account to any value name, I have programmatically
detected the account. In fact, this is a good PowerShell lesson to keep your scripts 
independent of assumptions (e.g. the Administrator account is named Administrator).  

## Resources 
* [Microsoft Learn - Security identifiers](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers)  

### Conclusion  
Renaming the Administrator account is security through obscurity. This, in turn, 
promotes bad security practices by those that need to get the work done. 
In conclusion, the rename of the local Administrator account is an ineffective security method.  

If you have no use for the Administrator account, then disable it: ```Disable-LocalUser -SID $SIDFromAbove ```  

Or if you do have a use for the account, then implement [LAPs](https://learn.microsoft.com/en-us/windows-server/identity/laps/laps-overview).  

Disagree with me? Feel free to share in the comments. I enjoy hearing your thoughts so I can continue to grow and learn.  