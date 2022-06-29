---
title: "Configure CCH Axcess SSO with Azure AD"
last_modified_at: 2022-06-28 21:01 
comments: true
tags: [Software]
excerpt: "Configure Single Sign-on with Microsoft Azure Active Directory for Wolter Kluwers CCH Axcess Application."
---

**Legacy Content Warning:** The post is now out-of-date (first posted on September 9, 2020). While it may still be helpful as a reference, 
please note the content and steps are missing critical steps to finish the process. As of today, I have no plans to update the information. 
With that said, I have kept this post as a historical reference. Public comments (if open) are always welcome at the bottom of the post to help the next person. 
For any configuration help, I recommend creating a support case with Wolters Kluwer. As always, I appreciate you stopping by!  
{: .notice--warning}  

Single Sign-on is a huge time saving, stress relief, and
secure choice to consider when using the CCH Axcess Tax Software. One possible choice
for the SSO configuration is Microsoft Azure Active Directory. The below guide will assist with this setup.

Wolters Kluwer has Solution ID [000108412](https://support.cch.com/kb/solution/000108412/How-do-I-configure-CCH-Axcess-ADFS-integration-using-Microsoft-Azure) to overview this setup.

**Quick Tip:** If you have any difficulties with the SSO setup in this guide or
in Wolters Kluwer knowledgebase, you may have to put in support a ticket to verify the
feature is enabled on your account.
{: .notice--primary}

# Setup Azure for CCH Axcess SSO
1. Login to the [Azure Active Directory Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).
2. Go to Enterprise applications > New Application
3. In the Add a Application menu, choose "Non-gallery" application
4. Add a name for your application. I entered "CCH Axcess Tax" > Add
5. In the manage grouping, select "Single Sign-on"
6. For the single sign-on method, select SAML
7. Scroll down to section 3, "SAML Signing Certificate." Download
the Certificate (Base64). The certificate will download as the name you used in
step 4. In our example, "CCH Axcess Tax.cer" The certificate will be used in a later step.  

 Stay signed in and on this screen to reference for later steps.

{:start="8"}
8. Launch CCH Axcess Dashboard > Choose Application Links
9. In the Firm grouping, choose Settings and defaults
10. In the resulting window, navigate to Login setup
11. Scroll to the bottom of the page, in the Federation Services settings, check
the box "Enable Pilot mode for Federation Services Login"
12. In the resulting window, click Browse... next to "Identity Provider certificate"
Navigate to the certificate found in step 7, "CCH Axcess Tax.Cer"
13. Referencing section 4 in the SAML-based Sign-on page, copy the URL for Azure AD Identifer and
paste the link into "Issuer" on the Federation Services settings.
14. The Entity ID should be a unique value that does not resolve. In this field,
I suggest putting https://*contoso*.cchaxcess.com where *contoso* is your domain.
15. Change the Federation type to Passive Federation  
16. Next, the SAML Single Sign On Service URL should be set to the value Logon URL (section 4).  

 To this point, your values should reflect the following:  
 ![CCH Axcess Values]({{ site.url }}{{ site.baseurl }}/images/cchaxcess/CCHAxcessValues-AzureAD.png)

{:start="17"}
17. Hit next  
18. Select Generate metadata and save it to your desired location  
19. Back on the SAML-based Sign-on configuration page (at the top), choose Upload metadata file. Browse to the location you saved the file in step 17 and choose Add.  
20. Click Save in the next window.  
21. If prompted to Test single sign-on with your app, choose No, I'll test later.
22. Next, on the Azure page, navigate from Single Sign-on to Users and Groups.
23. Assign the Azure Enterprise App to your beta Users by clicking Add user > Add individual account or
a group. Click Select. Click Assign.

## Testing Single Sign-On with your Pilot/Beta group  
At this stage, you are ready to test the configuration!

**Info** Before proceeding in testing, verify your pilot CCH Axcess staff accounts have an email address that match the attribute Mail in Azure Active Directory.
{: .notice--info}

On the workstation you wish to enable SSO:
1. Verify the Pilot/Beta user is assigned the Azure Enterprise App (step 22 in previous section)
2. Login to CCH Axcess Dashboard with the credentials you wish to test SSO
3. Go to Utilities  
4. Click on "Generate Federation Services Pilot Login Code"
5. On the next window, choose "Generate Pilot Login Code"
6. Click Close
7. Completely close the CCH Axcess Application
8. Launch and verify SSO configuration

**Info** To remove this configuration: Control Panel > **system** > **Advanced System Settings** > Advanced tab > Environment Variables > Remove variable **CCHPilotADFS**
{: .notice--info}

## Enable for all users

**Important** This can only be enabled by the **Default Axcess Administrator Account**
{: .notice--primary}

**Info** Before proceeding in site wide enablement, verify all CCH Axcess staff accounts have an email address that match the attribute Mail in Azure Active Directory.  
{: .notice--info}

1. Verify all CCH Tax User accounts are assigned to the Azure Enterprise App
2. Pilot SSO with Default Axcess Administrator Account
3. Open CCH Axcess Dashboard > Application Links > Settings and Defaults (Under Firm)
4. Select Login Setup
5. Select **Federation Services** as the login mode
6. Click Finish

### Final Words
A lot of additional configuration is not covered in this post that should be strongly considered.
For example, implementation of conditional access policies to prompt for two-factor verification. Since employees will be accessing confidential tax data,
I suggest reviewing applicable authentication security goals for your firm.

Anyways, I have to jet. Off for a five mile (eight kilometer) run. Hope you have a splendid day!
