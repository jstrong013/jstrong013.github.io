---
title: "You Named the Server what?!"
tags: [Opinion, Infrastructure]
excerpt: "HappyServer23 is not a good Server name. Change my mind." 
---
You are about to build a new server. Unfortunately, your coworker has provided a link to this post after you shared the server name. Why? 
The reason is because the server name is downright terrible. At best, the server name was meant for a home lab. The hostname of 
your server will later cause confusion, makes it hard to scale, and causes the IT Gods (both the old and the new) no mercy when issues arise.  

This is your warning to fix the d@mn hostname of the device.  Okay, with tough love out of the way, we will cover where you became misguided and 
provide a possible good domain name for your fresh, new server.  

## Terrible Server Name  
First, you landed on this post because you likely named your server with one of the following variations:  
* Some combination of letter and numbers that have no format or structure    
* A TV show or movie reference  
* Your favorite musical composer  
* Your coworkers name  
* **Localhost**  
* **local**
* Anything to do with the Galaxy, planets, or space  
* Star Wars related  
* Your favorite beers  
* ~~You pissed off your coworker (Apology cookies may fix this one)~~  

Okay, you get the gist. Naming a server with the criteria above creates ambiguity, confusion, and stress. While they *may* work for a home lab, they certainly should <ins>NOT</ins> be advised for use in an enterprise or business environment. 

To illustrate the point, say I named my server [E-Corp](https://mrrobot.fandom.com/wiki/E_Corp?utm_source=jstrong013.github.io&utm_medium=Blog&utm_campaign=Post). I have three senior Engineers on a critical case. I'm wasting valuable time sharing that E-Corp is down 
since not everyone will understand I'm referencing the Primary Domain Controller.

In an environments with complex architecture, trying to recall that E-Corp, FSociety, and RedArmy server roles is not a fun time. 
Beyond this, a solid naming convention for your servers allows for seamless automation, documentation, and simpler processes.   

If you disagree, I challenge you to describe in depth to a coworker your home environment with bad server names. Can they readily understand your infrastructure?   

## A Better Server Name  
A good server follows a well [documented](https://www.linkedin.com/pulse/become-legend-your-company-longer-than-employment-jeremiah-strong/) convention. The node name will give insights into the enterprise function, role, or concept that IT professionals may 
all understand. Over the years, I have thought of a few possible hostname conventions for various devices. For Servers, I like the following criteria you may choose to incorporate in your naming convention:  

* Environment (e.g. Production, Development, or Project)
* Location (e.g. City, Data Center)
* Primary service or application (e.g. Exchange, Configuration Manager Current Branch, File Server)
* Operating System (e.g. Windows, Linux)
* Type or function 
* Physical or Virtual  
* Unique number or count 


The abbreviations can for the above may be one, two, or three letter abbreviations using a consistent, well-known standard:  

* Production = P or PRD, Development = D or DEV  
* Minneapolis = MSP (Nearest Airport Code - [IATA](https://www.iata.org/en/publications/directories/code-search/?airport.search=msp "IATA Airport code for Minneapolis"))  
* United States of America = US (Country Code - [UN/LOCODE](https://unece.org/trade/cefact/unlocode-code-list-country-and-territory "UN/LOCODE for the United States") ) 
* Virtual = V, Physical = P

From here, it is relatively easy to create a server naming standard that works for your organization. This will be different depending on a multitude of factors:  

* Single site or multi-site business?  
* Data centers international?  
* Planning to scale or have large growth?  
* Abbreviations consistent or prevent confusion (i.e. P = Production or Physical)?
* Lucky to have more than one environment (umm, only Production here)?

To construct with the above criteria, here are a few possibilities:  

1. ```3 character Environment + 3 character Location + 1 character Physical/Virtual + 3 Character Server Type + 2 Character Count```
2. [Azure Cloud Framework](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming): ```Resource type | Workload Application | Environment | Azure Region | Instance``` 
3. ```Function + Unique Count```

This could translate as:  

1. ```PRDMSPVADC07``` &rarr; Production + Minneapolis/St.Paul + Virtual + Domain Controller + Server 07  
2. ```pip-sharepoint-prod-westus-001``` &rarr; (Azure Example)  
    > a public IP resource for a production SharePoint workload in the West US region

    <cite>Microsoft</cite> --- Define your naming convention
    {: .small}  

3. ```WEB001``` &rarr; Function + Unique Count

Going off our previous bad name illustrations, those servers were called E-Corp, FSociety, and RedArmy. As such, no longer do you have to do the extra mental energy deciphering E-Corp, FSociety, and RedArmy.  The name provides the insight.  

Before you name your server, be sure to have the standard documented!  
{: .notice--info}

### Helpful Resources  
Building a good naming convention is not easy. To come up with criteria and ideas for good server names, I 
referenced the following resources:  

* [Microsoft Cloud Adoption Framework - Define your naming convention](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming)
* [Microsoft Windows Server - Naming conventions in Active Directory for computers, domains, sites, and OUs](https://learn.microsoft.com/en-us/troubleshoot/windows-server/identity/naming-conventions-for-computer-domain-site-ou)  
* [MNX - A Proper Server Naming Scheme](https://mnx.io/blog/a-proper-server-naming-scheme/)  
* [vEducate.co.uk - Naming Conventions and Standards for Systems and Devices](https://veducate.co.uk/naming-conventions/)  

#### Final Thoughts  
I made this as slight joke. Or maybe it is more of a rant? From my eight years in IT, I do believe a good hostname is beneficial for Production and Development environments.  
It acts as a method of self-documentation. With that said, I'm always open to other opinions and thoughts. What are your server naming conventions? Any names that you have 
experienced that makes you question the IT field? Disagree with my evaluation? Feel free to share in the comments.  

Anyways, I'm back off to learning more about the backbone of all IT infrastructure, networking.  

Cheers,  
Jeremiah  