---
title: Extract MSI from CaseWare Connector Executable
comments: true
tags: [Software]
excerpt: Get the MSI executable for CaseWare Connector to make deployment easier!
---
In a [previous post]({{ site.baseurl }}{% post_url 2019-11-08-Extract-MSI-CaseWare-Working-Papers %}) we obtained the core software MSI for the base installation of CaseWare Working Papers. In this post, we are obtaining the MSI for [CaseWare Connector](https://documentation.caseware.com/2018/Connector/en/Content/Overview/c_Introducing_CaseWare_Connector.htm).

So, to extract the MSI from the executable from the exe, follow the steps below.

1. Login to the [MyCase](https://my.caseware.com/account) CaseWare Website
2. In the Downloads tab, download CaseWare Connector version you need.  
The file will be in the following format `ConnectorSetup.exe `
3. Go through the setup/installation.
4. Navigate to the following location in file explorer to obtain the MSI

`%localappdata%\Downloaded Installations\{Product Code}\CaseWare Connector 20XX (x86).msi`

As noted previously, verify you have all software dependencies before your deployment.

You are now ready to deploy with your favorite deployment software!

Cheers,  
Jeremiah
