---
title: "Visio 2019 Opens then Immediately Crashes"
comments: true
tags: [Software,Visio]
excerpt: "March 2022 Office Update crashes Microsoft Visio 2019 immediately upon opening a file."
---
When attempting to open a Visio file from a DFS share, it would appear to open for
about two seconds and then immediately crash. Even worse, when copying that file to my local
desktop it would still attempt to open the auto recover file rather than the local desktop file.

Standard troubleshooting methods **did not** work to resolve the situation:  
* Open Microsoft Visio application with no file  
* Launch Microsoft Visio in Safe Mode
* Run an Office Repair  
* Open a local file (the auto recover file would always be the file attempted to open)

# Workaround
[Procman](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) discovered the culprit.  To get Microsoft Visio 2019 to launch, you must first delete the auto recover file.  

Navigate to ```C:\Users\%UserProfile%\AppData\Local\Microsoft\Visio\``` and delete the
**AutoRecover11.ini** file.  

To allow the opening of the file, you will need to **Disable Enhanced Metafile optimizations**:

File > Options > Advanced (tab) > in the Display grouping, check the box next to "Disable Enhanced Metafile optimizations"

![Disable Enhanced Metafile optimizations]({{ site.url }}{{ site.baseurl }}/images/microsoft/visio/2022-03-28-disable-ehanced-metafile-optimizations.png)  
*Image courtesy of [Microsoft](https://support.microsoft.com/en-us/office/visio-crashes-after-installing-march-office-update-6b4edd7e-2a9a-4550-8f20-b98d3d4b596b).*  

## Resources  
A few days after encountering this issue, Microsoft released a support article.
* [Visio crashes after installing March Office update](https://support.microsoft.com/en-us/office/visio-crashes-after-installing-march-office-update-6b4edd7e-2a9a-4550-8f20-b98d3d4b596b)

First, thanks to Mark Russinovich for Procman. Second, enough IT for me today.  

Cheers,  
Jeremiah  
