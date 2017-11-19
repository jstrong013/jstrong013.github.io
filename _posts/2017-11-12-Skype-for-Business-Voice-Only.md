---
title: Skype for Business - Voice Only
tags: [Skype for Business]
excerpt: Voice Only presence in Skype For Business client.
---

At my place of employment, our phone system is Skype For Business. 
Overall, I really enjoy the Instant Messaging, Screen Sharing, and overall use 
of the software. 

However, at certain points throughout the day, I find that IM becomes a nuisance. That is, 
I want to focus on a project without the  ability of receiving an IM. As a technology professional, 
it is important that I keep myself available. As such, solutions such as Do Not Disturb mode, 
turning off IM notifications (with the ability of receiving IMs), or exiting the Skype for Business software are not viable to me. 
Rather, I would like my presence to say _Voice only_ to indicate phone calls only are desired at the current moment. 

## "Voice only" Presence
Unfortantely, as far as I am aware, no way exists to prevent other individuals from sending you an IM
while retaining your ability to communicate via IM. Furthermore, no graphical option is available to 
quickly turn off your IM capabilities. As an alternative option, a registry hack exists to disable IM
capabilities completely. The registry modificaiton in combination with PowerShell make this an easy
implementation.

To configure, save these two functions to your [profile.ps1.](https://technet.microsoft.com/en-us/library/bb613488(v=vs.85).aspx)

So, to disable IM in Skype For Business 2016:

    function Disable-S4BIm { 
        New-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Office\16.0\Lync" -Name "DisableIM" -PropertyType DWORD -Value 1 -Force
    }

Then, to enable IM in Skype For Business 2016:

    function Enable-S4BIm {
        Set-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Office\16.0\Lync" -Name "DisableIM" -Value 0 -Force
    }

*Quick Note*: In order for this to apply, you need to sign-out and sign-in to skype. 

Now, to disable IM capabilities in your Skype for Business client you can simply launch PowerShell as 
Administrator and run:

    Disable-S4BIm

With a quick sign-out and sign-in, you are now "Voice Only" capable! 

To restore IM cabilities:

    Enable-S4BIm

For registry modificaitons on different versions of office, simply change the path number:

    "HKLM:\Software\Policies\Microsoft\Office\*number*\Lync"

Where '*number*' is:

| Office Version | Number |
|--------------- | ------ |
| Office 2016    | 16.0   |
| Office 2013    | 15.0   |      
| Office 2010    | 14.0   |     


Happy phone calling! 

Jeremiah