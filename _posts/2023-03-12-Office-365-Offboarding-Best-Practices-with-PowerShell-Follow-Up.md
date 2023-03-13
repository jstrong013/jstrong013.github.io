---
title: "Office 365 Offboarding Best Practices with PowerShell Follow Up"
comments: true
tags: PowerShell
excerpt: "A brief follow up to Robert's post on Office 365 Offboarding with more PowerShell"
---
It is starting to get late and I am randomly surfing the internet. After reading some of my favorite bloggers, I stumbled across a random post, [Office 365 Offboarding Best Practices](https://blog.admindroid.com/office-365-offboarding-best-practices/?utm_source=jstrong013&utm_medium=ExternalBlog&utm_campaign=Post), by Robert. The post covers a lot of great 
material. With that said, I am a strong believer in automation.  While Robert shared how to do this graphically, I am sharing how to do the steps 
via PowerShell. After all, I love PowerShell.  With that said, I'm only sharing the one-liner commands (if possible).  

## 1. Logout User from All Office 365 Sessions   
```powershell
Get-AzureADUser -Filter "userPrincipalName eq 'PrevEmployeeUPN@company.com'" | Revoke-AzureADUserAllRefreshToken 
```

Microsoft documentation may be found here:  
* [Revoke-AzureADUserAllRefreshToken](https://learn.microsoft.com/en-us/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0)
* [Get-AzureADUser](https://learn.microsoft.com/en-us/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

## 2. Block Account Sign-in and Reset Password  

To block the account from authentication: 

```powershell
Set-AzureADUser -ObjectID PrevEmployeeUPN@company.com -AccountEnabled $false
```

Resetting an account password can be done via PS: 

```powershell
Set-MsolUserPassword –UserPrincipalName "PrevEmployeeUPN@company.com" –NewPassword "Considering-Doing-A-Very-Secure-Passphrase-100#" -ForceChangePassword $False
```

Microsoft documentation: 
* [Set-AzureADUser](https://learn.microsoft.com/en-us/powershell/module/azuread/set-azureaduser?view=azureadps-2.0)  
* [Set-MsolUserPassword](https://learn.microsoft.com/en-us/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)  

## 3. Setup Email Forwarding  

```powershell
Set-Mailbox -Identity "PrevEmployeeUPN@company.com" -ForwardingAddress "Manager@company.com"  
```
Microsoft docs: 
* [Set-Mailbox](https://learn.microsoft.com/en-us/powershell/module/exchange/set-mailbox?view=exchange-ps)

## 4. Convert User Mailbox to Shared Mailbox  

```powershell
Set-Mailbox PrevEmployeeUPN@company.com -Type Shared
```
**Important:** Please keep in mind that this does not remove the license. At the time this post was published, shared mailboxes do not require a license. See step 10 to remove license.  
{: .notice}  

Microsoft docs: 
* [Set-Mailbox](https://learn.microsoft.com/en-us/powershell/module/exchange/set-mailbox?view=exchange-ps)  

## 5. Preserve former employees’ mailbox data  
In the blog post, three options were provided to preserve mailbox data.  

For option one, scripting to convert a mailbox to download a PST file is beyond a simple command.  

For option two, to set a Litigation hold on the mailbox:  
```powershell
set-mailbox -Identity PrevEmployeeUPN@company.com -LitigationHoldEnabled $true 
```  

To transition to option three, inactive mailbox, you have to wait for the retention policy to take affect on the mailbox (generally about four hours). After this stage, delete the mailbox (see step 11).  

Microsoft docs: 
* [Set-Mailbox](https://learn.microsoft.com/en-us/powershell/module/exchange/set-mailbox?view=exchange-ps)  
* [Create and manage inactive mailboxes](https://learn.microsoft.com/en-us/microsoft-365/compliance/create-and-manage-inactive-mailboxes?view=o365-worldwide)  

## 6. Transfer email alias  

As per the blog, the assumption is the account no longer exists in the tenant. To add the email address to another employee, you would add it as an alias.  

```powershell  
Set-Mailbox -Identity manager@company.com -Emailaddresses @{Add='PrevEmployeeEmail@company.com'}  
```

Microsoft docs: 
* [Set-Mailbox](https://learn.microsoft.com/en-us/powershell/module/exchange/set-mailbox?view=exchange-ps)  

## 7. Move leavers’ OneDrive data to other location   
For this one, no one-liner exists. Elliot Munro has us covered with his solution, [Transfer all OneDrive files to another user via PowerShell](https://gcits.com/knowledge-base/transfer-users-onedrive-files-another-user-via-powershell/?utm_source=ExternalBlog&utm_medium=Link&utm_campaign=Post).  

## 8. Wipe and block the user’s mobile device  
Once again, a full script would have to be established. With that said, you could use the Graph API. 

```powershell
Invoke-RestMethod -Uri "https://graph.microsoft.com/v1.0/deviceManagement/managedDevices/$ManagedDeviceID/wipe" -Method Post -Headers $AuthorizationToken 
```

Microsoft Docs:
* [Graph API - wipe action](https://learn.microsoft.com/en-us/graph/api/intune-devices-manageddevice-wipe?view=graph-rest-1.0)  
* [Invoke-RestMethod](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-7.3)  

## 9. Remove user from all groups  
I'll leave a reference to Salaudeen Rajack post, [How to Remove a User from Office 365 Group using PowerShell?](https://www.sharepointdiary.com/2019/01/remove-user-from-office-365-group-using-powershell.html).  


## 10. Remove license   
Honestly, PowerShell is not always the best option. In my opinion, this is one of those cases. If you have a P1 and Microsoft E3 license, then you can leverage group based licensing. 

With group based licensing, the removal of the User from the group (that happened in #9) means you already removed the license!  

If you must use PowerShell then checkout Microsoft Docs: [Remove Microsoft 365 licenses from user accounts with PowerShell](https://learn.microsoft.com/en-us/microsoft-365/enterprise/remove-licenses-from-user-accounts-with-microsoft-365-powershell?view=o365-worldwide).  

Microsoft Docs: 
* [What is group-based licensing in Azure Active Directory?](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)  


## 11. Delete account  

```powershell
Remove-AzureADUser -ObjectID PrevEmployeeUPN@company.com
```

Or you can also do through the Microsoft Azure Active Directory Module for Windows PowerShell:  
```powershell
Remove-Msoluser -UserPrincipalName PrevEmployeeUPN@company.com
```  

Microsoft Docs:
* [Remove-AzureADUser](https://learn.microsoft.com/en-us/powershell/module/azuread/remove-azureaduser?view=azureadps-2.0)  
* [Remove-MsolUser](https://learn.microsoft.com/en-us/powershell/module/msonline/remove-msoluser?view=azureadps-1.0)

### Final Thoughts  
A lot is missing in this post. The basics of a full featured PowerShell script exist somewhere in the missing details. 

If you create this script and post it on Github (or create a blog post), please drop a link 
in the comments.  

Also, are you doing the steps Robert outlined? Can you think of anymore? A big shoutout to Robert for a concise and important post on 
a subject matter that warrants attention for any size business.  

Anyways, time to hit the sack (I mean watch the Season One finale of The Last of Us).  

Cheers,  
Jeremiah  