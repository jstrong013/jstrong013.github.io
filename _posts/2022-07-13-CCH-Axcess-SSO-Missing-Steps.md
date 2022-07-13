---
title: "CCH Axcess SSO Missing Steps"
comments: true
tags: [Software]
excerpt: "A post that identifies common configuration issues from a previous blog post on setup and configuration of Azure SSO for the CCH Axcess Application."
---
Earlier this month, I received an email from a lovely soul named Lauren. She informed me that she referenced my previous 
post, ![Configure CCH Axcess SSO with Azure AD]( {% post_url 2020-09-06-CCH-Axcess-SSO-with-Microsoft-Azure-AD.md %}/?utm_source=InternalBlog&utm_medium=LinkReference&utm_campaign=BlogPost ). Unfortunately, she 
ran into some configuration issues.  

After a seven minute screenshare, the issues on the configuration were discovered.  

**Note:** The information contained in this post worked for us on June 10, 2022. As with most things in Information Technology - it changes. Please keep 
this in mind when referencing this post.  
{: .notice--primary}  

# Issue 1: Reply URL not populated  
The Reply URL (Assertion Consumer Service URL) should be set to the following value as the default : ```https://api.cchaxcess.com/RelyingPartySTS/RelyingParty.aspx```  

![CCH Axcess Reply URL]({{ site.url }}{{ site.baseurl }}/images/cchaxcess/2022-06-07_22_03_50-ReplyURL.png)  

The Reply URLs ideally should be populated from the upload of the metadata file. The values may be found by opening the metadata file inspecting the AssertConsumerService element.  

For Lauren, the values had to be manually added.  

![CCH Axcess Reply URL from metadata file]({{ site.url }}{{ site.baseurl }}/images/cchaxcess/2022-06-07_22_09_48-ReplyURLXML.png)  

## Configuration
1. To set the Reply URL, login to the [Azure Portal](https://portal.azure.com).  
2. Navigate to Enterprise Applications > Search for your CCH Axcess Application.  
3. In the managing grouping, choose Single sign-on.  
4. In section 1, Basic SAML Configuration, click the pencil icon to edit  
5. Input the value ```https://api.cchaxcess.com/RelyingPartySTS/RelyingParty.aspx``` in the Reply URL and mark as default  
6. *(Optional)* Input the remaining URLs from the metadata XML file 

# Issue 2: Attributes & Claims were incorrect  

**Warning:** Failure to do these steps will result in login failures *after* Federation Services Login method is turned on for the entire account. 
During pilot, your authentications may work fine. 
The error message will be 'We are unable to log you in, please contact your admin, your private personal identifier PPID does not match your profile in CCH Axcess Tax."     
{: .notice--warning}  

The required claim should be set to Value ```user.mail```.  

![CCH Axcess required claim]({{ site.url }}{{ site.baseurl }}/images/cchaxcess/2022-06-07_17_54_38-CCHAxcessRequiredClaim.png)  

For additional claims, the private personal identifier claim needs to be created with the following values:  

| Name         | Namespace                                                                       | Source    | Source Attribute | 
| ---          | ---------                                                                       | ------    | ----------       | 
| emailaddress | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier | Attribute | user.mail        |  

![CCH Axcess required claims]({{ site.url }}{{ site.baseurl }}/images/cchaxcess/2022-06-07_22_26_12-PPIDClaim.png)  

The above assumes that you are using email address as the identifier thus referencing the mail attribute in Azure AD.    

**Note:** To verify your Claim type in CCH Axcess: Launch CCH Axcess Dashboard > Click Application Links > In the Firm grouping, click Settings and defaults > In the new window, choose Login Setup > Click the link View current Federation Services settings > In the next window, review the 'Claim type'.  
{: .notice--info}  

In Lauren's case, it was set to **Staff System Email address**.  

![CCH Axcess claim type]({{ site.url }}{{ site.baseurl }}/images/cchaxcess/2022-06-07_17_03_10-ClaimType.png)  

After updating for Lauren, authentications were seamless.  

## Configuration  
1. To set the Reply URL, login to the [Azure Portal](https://portal.azure.com).  
2. Navigate to Enterprise Applications > Search for your CCH Axcess Application.  
3. In the managing grouping, choose Single sign-on.  
4. In section 2, Attributes & Claims, click the pencil icon to edit  
5. Click on the required claim and update to appropriate  
6. Next, update the additional claim that is associated to ```user.mail``` or add a new claim  

### Resources 
* [We are unable to log you in, please contact your admin, your private personal identifier PPID does not match your profile in CCH Axcess™ Tax.](https://support.cch.com/kb/solution/000107813/Internal-Only-Error-We-are-unable-to-log-you-in-please-contact-your-admin-your-private-personal-identifier-PPID-does-not-match-your-profile-in-CCH-Axcess)  
* [How do I configure CCH Axcess™ ADFS integration using Microsoft® Azure?](https://support.cch.com/kb/solution/000108412/How-do-I-configure-CCH-Axcess-ADFS-integration-using-Microsoft-Azure)  

#### Time to go paddle boarding  
Hopefully this post helps with your CCH Axcess Tax Single sign-on setup. After all, my entire aim of this blog is to assist the community after 
referencing hundreds of blogs. In essence, a small way to give back. 
If you would like assistance, would like to share a link to your blog, or would like to 
share that this post helped you, please post a comment. I'm honestly appreciative that the content helps anyone. Lauren (and all others), thanks for letting me 
now that you have found value on these posts.  
Okay, I am on holiday this week. In other words, I believe it is time to create an adventure to a new lake for a paddle boarding experience.  

Until the next post,  
Jeremiah  