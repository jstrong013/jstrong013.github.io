---
title: Extract MSI from CaseWare Working Papers Executable
comments: true 
tags: [Software]
excerpt: Get the MSI executable for CaseWare Working Papers software to make deployment easier!
---
CaseWare International does not provide the MSI for CaseWare Working papers.
While this does make sense for normal installation, it is not helpful for us Administrators
who use System Center Configuration Manager or other deployment tools.

So, to extract the MSI from the executable from the exe, follow the steps below.

1. Login to the [MyCase](https://my.caseware.com/account) CaseWare Website
2. In the Downloads tab, download CaseWare Working Papers version you need.  
The file will be in the following format `WP20XXUSSYNCX64_<AuthCode>_.exe `
3. Double click the executable but do NOT go through the setup
4. Navigate to the following location in file explorer to obtain the MSI

`%localappdata%\Downloaded Installations\{Product Code}\CaseWare Working Papers 20XX (x64).msi`

Note: The `{Product Code}` will change for the Year/Version of Working Papers you have downloaded.

On a final mention, the msi will not install dependencies that are required for
the software to run that would otherwise be installed if running the exe. Be sure to
include these dependencies in your roll out for a smooth deployment.

The failure to install dependencies would result in [Error 1904](https://documentation.caseware.com/2018/WorkingPapers/en/Content/CaseWareWP/Getting_Started/Installation/r_Troubleshooting.htm) for example.
> * Error 1904. Module C:\Program Files\CaseWare\CWPCore.dll failed to register. HRESULT -2147220473. Contact your support personnel.
> * The program can’t start because MSVCR100.dll is missing from your computer.

Anyways, I'm working on helping my organization adopt [CaseWare Cloud](https://docs.caseware.com/2019/WebApps/30/en/Explore/Getting-Started/Getting-started-guide.htm). With a multi-office setup
and thousands of files to migrate, I gotta jet.

Cheers,  
Jeremiah
