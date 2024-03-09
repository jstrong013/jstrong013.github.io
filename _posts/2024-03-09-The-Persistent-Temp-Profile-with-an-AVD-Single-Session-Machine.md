---
title: "The Persistent Temp Profile with an AVD Single Session Machine"
comments: true
tags: [Azure,Azure Virtual Desktop]
excerpt: "On every login of an Azure Virtual Desktop Single Session machine, the user has a temp profile. What gives?!" 
---
Hey folks,  

So, picture this: you're setting up an Azure Virtual Desktop environment, everything seems to be going smoothly, until suddenly, you hit a roadblock. Every time a user logs in, they 
are greeted with this pesky message:  

> We can't sign in to your account: This problem can often be fixed by signing out of your account and then signing back in. If you don't sign out now, any files you create or changes you make will be lost.  

![We can't sign in to your account]({{ site.url }}{{ site.baseurl }}/images/azurevirtualdesktop/2024-03-wecantsignintoyouraccount.png)  

Unsurprisingly, a log off and login did not fix a thing.  

Off to the [Microsoft documentation](https://learn.microsoft.com/en-us/troubleshoot/azure/virtual-machines/troubleshoot-rdp-cannot-sign-into-account#cause) I go, hoping for some insight.
According to their wisdom, a corrupt user local profile is to blame for this issue. But in our case, that wasn't the smoking gun as swapping out the user to a brand-new AVD machine didn't do the trick.  

So, it became clear: the problem wasn't with the virtual machine itself, but something deeper.   

## The Great Mystery Solved  
After some more digging, I peeked into the user's Active Directory profile. Lo and behold, in 
the User's profile tab, in the box next to 'Profile path:' was some rogue information that was meant for the 'logon script' section. 
After the fix, all logins were back to operational status.  

### Resources for Your Troubleshooting Adventures    
If you're diving into setting up your own Azure Virtual Desktop playground and want to 
learn from your own customizations of the environment, check out [Configuring and Operating Microsoft Azure Virtual Desktop (AZ-140)](https://www.pluralsight.com/paths/configuring-and-operating-microsoft-azure-virtual-desktop-az-140-2023) by [Ned Bellavance](https://nedinthecloud.com/). 
His [Terraform code](https://github.com/ned1313/Implement-an-AVD-Infrastructure) makes spinning up a test environment a breeze. Plus, it's perfect for breaking, fixing, and testing to your heart's content.  

#### Until Next Time   
So, what's your "How did I miss that?!" moment in Azure Virtual Desktop land?  
Share your tales of triumph or woe, and let's commiserate or celebrate together.  

Anyway, enough tech talk for now. I'm off for a much-needed run.  

Cheers,  
Jeremiah
