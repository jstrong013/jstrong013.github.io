---
title: "Citrix Files for Outlook Fired an Exception"
comments: true
tags: [Software]
excerpt: "The Citrix Files for Outlook plug-in immediately prompts with the error
message: Citrix Files for Outlook Fired an Exception. Here is my resolution without
deleting the Windows User Profile."  
---
Upon launching outlook, one of my colleagues was met with the following error message:
'Citrix Files for Outlook' has fired an exception. Click the 'Details' button to see
the detailed information about the error.  
 ![Citrix Files for Outlook Fired an Exception]({{ site.url }}{{ site.baseurl }}/images/sharefile/CitrixFilesFiredException.png)

I reached out to Citrix support who suggested to rebuild the Outlook profile and/or
recreate the Windows User Profile.

While the above solution would have worked, it was a last resort option in this case.   

Since Citrix support was unable to resolve, I ran Procmon, a windows monitoring tool, to determine the issue and resolution.    

## Solution  
To fix this error in my instance, I took the following steps:

1. Close Outlook
2. Navigate to 'C:\Users\\*Username*\AppData\Roaming\Citrix\Citrix Files for Outlook'
3. Delete all files in this directory. Specifically, you should delete **settings.cfg**
4. Launch Outlook and authenticate the Citrix Files for Outlook Plug-in  

The error message should no longer prompt!

### Common resolutions  
While the above prevented me from having to delete the Windows profile, the following
may also have to be performed. At the very minimum, the following solves most Citrix Files
for Outlook issues:
* Verify Add-in is not [disabled](https://support.citrix.com/article/CTX207690)  
* Set the Outlook registry key to always enable Citrix Files for Outlook add-in  
* Uninstall/Install the Citrix Files for Outlook add-in    
* Review the [known issues list](https://support.citrix.com/article/CTX207689)  


Well friends, hope this was helpful.  
Cheers,  
Jeremiah  
