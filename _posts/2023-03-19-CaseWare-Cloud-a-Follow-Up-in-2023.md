---
title: "CaseWare Cloud: A Follow Up in 2023"
comments: true
tags: [Software, Opinion]
excerpt: "A follow up to the post 'CaseWare Cloud: the Good, the Bad, the Meh' on the developments to Cloud 31.0"
---
In a [previous post]({{ site.url }}{{ site.baseurl }}{% post_url 2020-05-07-CaseWare-Cloud-Good-Bad-Meh %}?utm_source=InternalBlog&utm_medium=LinkReference&utm_campaign=BlogPost), 
I shared a few basic opinions of CaseWare Cloud. 
Since then, CaseWare Cloud Engineers have been working diligently in improving 
the SaaS service.  Here is a very quick and brief follow-up to my prior post.  

## What's Improved?    
The big improvements include:  
* You can integrate Single Sign-on with [ADFS](https://docs.caseware.com/2020/webapps/31/en/Setup/Environments-and-Configuration/Integrate-single-sign-on-with-ADFS.htm) or [Azure](https://docs.caseware.com/2020/webapps/31/en/Setup/Environments-and-Configuration/Integrate-single-sign-on-with-Azure-AD.htm)! 
  * Be sure to configure Multifactor Authentication on your identity provider for strong security     
* A [Cloud API](https://docs.caseware.com/2020/webapps/31/en/Caseware-Cloud-API/query-data-using-get-requests.htm) allows you to create and delete staff (among other operations)  
* [Business Units](https://docs.caseware.com/2020/webapps/31/en/Explore/Interface/Firm-Settings/Firm.htm) allow you to have better data access controls  

## Still the Same   
A few things remain the same:  
* No integration with Active Directory for User creation/removal
  * Minor point as the Cloud API option now exists    
* No integration or options to use your backup software to protect your firm data  
  * [Paid data recovery options](https://docs.caseware.com/2020/webapps/31/en/Practice/Maintenance/Data-recovery-limitations.htm) from CaseWare are available  

## Would Like to See   
The improvements are very exciting to see. CaseWare is getting large firm attention 
with the [AICPA getting involved](https://www.aicpa.org/news/article/aicpa-and-cpa-launch-onpoint-ebp-on-caseware-cloud-platform). From an enterprise management perspective, 
specifically on System Administration side, I hope to see:  
* A CaseWare Working Papers online authorization revoke feature. Since CaseWare Cloud only operates in a sync environment for files, I would love to see a revoke license to CaseWare Working Papers desktop client
  * If you handle thousands of workstations, this will bring on massive resource overhead   
  * You can see a list of authorizations now via [My CaseWare portal](https://my.caseware.com)!   
  * The closest option to *remotely* [revoke a license](https://documentation.caseware.com/2020/WorkingPapers/en/Content/Setup/Environments-Configuration/Licenses-Registration/Revoke-License.htm) is via command line: ```cwin32.exe /revokelicense```
* Community or CaseWare examples of Cloud API solutions  
  * This *may* be due to CaseWare agreements. The [CaseWare Developers](https://developers.caseware.com/) site has API documentation behind a login  


### Final Thoughts  
CaseWare Cloud has come a long way in a short amount of time. While I do not use the 
software in a business capacity, from a System Administration standpoint, the developments are great to see.  

Also, the Cloud API is a great addition! If you find public 
documentation that indicate community API projects are allowed to be posted online 
or have insight of a community project to share (ideally PowerShell and CaseWare Cloud Staff/Group management), 
then please let me know in the comments below!  

While this is brief follow-up post, you can explore more feature changes in the Cloud 31.0 version by visiting: [What's New Cloud 31.0](https://docs.caseware.com/2020/webapps/31/en/Explore/Whats-New/whats-new-cloud-31.htm).
At this time, I believe the latest version is Cloud 32!  

I wish you luck in CaseWare Cloud journey!  