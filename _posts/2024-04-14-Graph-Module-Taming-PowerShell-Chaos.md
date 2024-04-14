---
title: "Graph Module: Taming PowerShell Chaos"
comments: true
tags: [Software,PowerShell]
excerpt: "Facing exceptions with Microsoft Graph PowerShell? It's time for a scorched earth approach."
---
As the tech world spins faster with each passing day, our toolkit continually evolves. 
With Microsoft phasing out several legacy modules in favor of the more comprehensive Microsoft Graph PowerShell module, our reliance on this newer, more versatile tool has never been greater. 
But what happens when this central cog in your PowerShell setup throws a curveball, confronting you with vexing errors like these?

```Void Microsoft.Graph.Authentication.AzureIdentityAccessTokenProvider..ctor(Azure.Core.TokenCredential```  

or  

```Could not load type 'Microsoft.Graph.Authentication.AzureIdentityAccessTokenProvider' from assembly```  

or  

```Error: The '<Your Cmdlet>' command was found in the module 'Microsoft.Graph.<The Module>', but the module could not be loaded.```  

This infuriating exception for any Microsoft Graph PowerShell Cmdlet will most likely be resolved by upgrading the Graph Module 
to the latest version. 
If you come along to this blog post, you likely have already given this a try by 
executing `Update-Module Microsoft.Graph -Force` or by installing the latest version `Install-Module Microsoft.Graph`. 

But what if you've already tried that and you're still playing whack-a-mole with errors?

## The Clean Slate Approach  
If your PowerShell environment doesn't need to cling to any specific version of the Graph module, then it's time for the "scorched earth" strategy—wipe everything clean and start afresh.  

**INFO:** To prevent a warning message of the module being 'in use', close all open PowerShell windows and Editors (e.g. Visual Studio Code) before running the below scripts.
{: .notice}

Here’s a handy script I prefer, crafted by [Andres Bohren](https://blog.icewolf.ch/about/aboutme/), which neatly uninstalls all versions of the Microsoft Graph Module

```powershell  
<#
.NOTES 
    Andres Bohren
.LINK 
    https://blog.icewolf.ch/archive/2022/03/18/cleanup-microsoft-graph-powershell-modules/
#>

$Modules = Get-Module Microsoft.Graph* -ListAvailable | Where {$_.Name -ne "Microsoft.Graph.Authentication"} | Select-Object Name -Unique
Foreach ($Module in $Modules)
{
    $ModuleName = $Module.Name
    $Versions = Get-Module $ModuleName -ListAvailable
    Foreach ($Version in $Versions)
    {
        $ModuleVersion = $Version.Version
        Write-Host "Uninstall-Module $ModuleName $ModuleVersion"
        Uninstall-Module $ModuleName -RequiredVersion $ModuleVersion
    }
}
#Uninstall Microsoft.Graph.Authentication
$ModuleName = "Microsoft.Graph.Authentication"
$Versions = Get-Module $ModuleName -ListAvailable
Foreach ($Version in $Versions)
{
    $ModuleVersion = $Version.Version
    Write-Host "Uninstall-Module $ModuleName $ModuleVersion"
    Uninstall-Module $ModuleName -RequiredVersion $ModuleVersion
}
Install-Module Microsoft.Graph
```
This script is a bit more verbose and gives you a progress update, which is quite satisfying in today’s instant-gratification world.

If you prefer a more documented approach as outlined by [Microsoft](https://learn.microsoft.com/en-us/powershell/microsoftgraph/installation?view=graph-powershell-1.0#uninstalling-the-sdk), you could also go with:  

```powershell
<#
.NOTES 
    Below is from Microsoft Docs (now known as Microsoft Learn).
.LINK
    https://learn.microsoft.com/en-us/powershell/microsoftgraph/installation?view=graph-powershell-1.0
#>
# First, use the following command to uninstall the main module.
Uninstall-Module Microsoft.Graph -AllVersions
# Then, remove all of the dependency modules by running the following commands.
Get-InstalledModule Microsoft.Graph.* | ? Name -ne "Microsoft.Graph.Authentication" | Uninstall-Module -AllVersions
Uninstall-Module Microsoft.Graph.Authentication -AllVersions

# Install Again 
Install-Module Microsoft.Graph
```

### More issues?   
If burning everything to the ground doesn't smooth things over, the [Microsoft troubleshooting guide](https://learn.microsoft.com/en-us/powershell/microsoftgraph/troubleshooting?view=graph-powershell-1.0) is a good place to visit.  

And with that, I'm off to grab some delicious tacos! 
Here's to fewer errors and more efficient scripting days ahead! 

Happy coding, everyone!  
Jeremiah   


