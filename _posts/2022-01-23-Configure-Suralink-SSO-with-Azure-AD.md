---
title: "Configure Suralink SSO with Azure AD"
comments: true
tags: [Software]
excerpt: "Configure Single Sign-on with Microsoft Azure Active Directory for Suralink."
---
Single Sign-on (SSO) is a fantastic idea for Suralink. The below guide will assist with SSO setup
for Suralink with Azure Active Directory.

**Before we begin:** The Azure environment updates [frequently](https://azure.microsoft.com/en-us/updates/). The screenshots and
information is up-to-date as of January 2022.
{: .notice}

# Licensing Requirements  
At the time of this writing, single Sign-On is a [Premium/Enterprise feature](https://www.suralink.com/pricing).  
Unfortunately, this means that you will need to [upgrade your licenses](https://stopthesso.tax/) if you are on the Standard plan.

# Before you begin  
To perform the operations below, you must be an Azure [Application Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#application-administrator) or [Global Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#global-administrator).
In the Suralink platform, you must be an Administrator.  

Finally, team profiles (aka Employees) in the Suralink system must have emails that match their Azure AD Profile email.  

**Info:** If you find incorrect emails in Suralink, you will have to reach out to [Suralink support](https://www.suralink.com/contact-us)
to get them updated.  
{: .notice--primary}

# Setup an Azure Enterprise Application and Configure SSO in Suralink  
1. Login to the [Azure Active Directory Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).
2. Go to Enterprise applications > New Application
3. Choose '+ Create your own application'
4. Add a name for your application. I entered "Suralink." Keep the radio box "Integrate any other application you don't find in the gallery (Non-gallery). Select Create.  

Keep this web page open for use in later steps.  

{:start="5"}
5. Login to your Suralink site as an Administrator.  
6. Go to 'My Firm' > Security tab  
7. In the Single Sign On (SSO) grouping, choose the option Enable Single Sign On.  

Keep this web page open to use in the subsequent steps.  

**Info:** At the time of this writing, their is a FAQ sheet available
for download in this section of the website by clicking the 'Suralink_SSO.pdf' URL.  
{: .notice--info}  

{:start="8"}
8. Go back to the Enterprise Application you created (the webpage you left open in step 4). Choose 'Single sign-on'  
9. Select 'SAML'  
10. In the section 1, "Basic SAML Configuration" click the pencil icon.  
11. Enter the generated values from Suralink (generated in step 7) to your Azure App.  
The following fields will be put in section 1:  
* Identifier (Entity ID)  
* Reply URL (Assertion Consumer Service URL)  
* Relay State  
* Logout URL  
It will look like the following:  

![Azure Section One Values]({{ site.url }}{{ site.baseurl }}/images/suralink/Suralink_AzureAD_SAML.png)  

After you hit save, it will look like the following:  

![Suralink and Azure Side By Side Values]({{ site.url }}{{ site.baseurl }}/images/suralink/Suralink_AzureAD_SAML_SidebySide.png)  

{:start="12"}  
12. Still in the Azure SAML configuration, go to section 3. Copy, download, and retain
the following items:  
* Federation Metadata XML URL  
* The Certificate (Base64)  

![Azure Section Three Values]({{ site.url }}{{ site.baseurl }}/images/suralink/Suralink_AzureAD_SAML_Certificate.png)  

{:start="13"}  
13. Next, in the Azure SAML configuration section 4, copy and retain the following URLS:  
* Login URL  
* Azure AD Identifier  

![Azure Section Three Values]({{ site.url }}{{ site.baseurl }}/images/suralink/Suralink_AzureAD_SAML_URLs.png)  

{:start="14"}  
14. Now, go back to the Suralink Admin page and paste the values
you captured in steps 12 and 13 into the appropriate boxes. For the
certificate, open this in Notepad and copy the physical text.  

![Suralink Identity Provider Values]({{ site.url }}{{ site.baseurl }}/images/suralink/Suralink_AzureIdentityProviderValues.png)  

{:start="15"}  
15. In the Suralink app, assign the identities you want to allow SSO by going to Users and Groups > adding the identities **OR** allowing everyone to authenticate by going to the Properities of the app > toggle 'Assignment Required' to 'No'  
16. That's it! You are ready to test!  

**Warning:** If you have Multi-Factor enabled in Suralink, disable this value by going to Suralink > My Firm > Security (tab) > Required Two-Factor Authentication section, select 'Disable Forced Two-Factor Authentication.' This will allow you to configure MFA on the Azure Identity Provider and prevent confusion on which MFA Provider they are using.  
{: .notice--warning}


## Employee Experience - Testing your SSO  
When you go to your Suralink site (https://contoso.suralink.com), it will update the login window.
Employees will now choose 'Authenticate With Single Sign On Provider'.
From here, they will be redirected to use their Azure credentials.  

![Suralink Login Window]({{ site.url }}{{ site.baseurl }}/images/suralink/Suralink_LoginExperience.png)  

## Client Experience  
Clients should go to https://app.suralink.com. If staff members have been providing
the company URL, https://contoso.suralink.com, then the client will have to choose
the link 'Back to username login form' to authenticate. With that said, if the client
should go back to https://contoso.suralink.com they will not have to click the link again.
That is, as long as they do not clear their browsers cache/cookies.  

### Final closing comments
The Azure Identity Provider has a ton of features! Please do the bare minimum of enabling
Multi-Factor Authentication via [conditional access policies](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-all-users-mfa)
or tenant [security defaults](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/concept-fundamentals-security-defaults). While it
is out of the scope of this post to cover the massive security features in Azure, I do strongly
encourage you to research and implement them as soon as possible due to the high level of
cybercrime over the last few years.  

Anyways, I'm off to setup an [Azure AD Application Proxy](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy) to
publish our Internal applications to be accessible from anywhere. I hope this post was helpful. If not,
Suralink support (at the time of this writing) is stellar!  

Peace,   
Jeremiah  
