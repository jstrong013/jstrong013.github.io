---
title: Why is this email going to Quarantine - SPF!?
tags: [Office 365, Exchange Online]
excerpt: Email going to quarantine due to incorrect SPF records.
comments: true
---

Instant messaging, video conferencing, and text messages are all tools that are replacing previous methods of communication. However, email correspondence is still
a popular choice heavily adopted in the business world. So, if an important email finds itself in a quarantine or a junk filter location the impact may be monumental. 

While many reasons exist for email correspondence to be redirected to junk locations, I am going to focus on one standard that is implemented by the Sender organization, called
Sender Policy Framework (SPF). 
Due to SPF being implemented by the sender organization, if it is configured incorrectly, legitimate mail may be sent to quarantine/junk by the receiving mail organization 
simply by following the rules defined by the sending organization. As such, the receiving organization's IT department is stuck in a tough predicament: create an exception for the senders
domain thus weakening email security for the organization and organizations at large OR continue to have mail flow to quarantine in the hopes that the sending organization updates their SPF
records to allow for proper mail flow. 

If you have reached this post, it may be the latter that is being suggested.  

**Standard Disclaimer**: This information is given for technical education only. It may not work for 
your specific situation. It is not professional advice, and I am not a certified professional with Office 365
or Exchange Online. You may have to find and hire a Microsoft certified professional to get technical advice 
to help assist with this topic. Last, the opinions expressed here represent my own ideas and thoughts at this moment and time.
They do not reflect those of my current employer. Furthermore, my view points do change over time as I continue to grow and learn. I may 
no longer agree with my own ideas but will retain this post for a historical archive.

# What is Sender Policy Framework?
The [Sender Policy Framework](http://www.openspf.org/), or SPF, is an email validation check to prevent against an impersonation of an email from another organization. 
In a sentence, SPF is a method that allows a company to identify to the globe which mail servers they authorize to send mail on their behalf.

To illustrate this concept, we will use three companies/individuals: 

1. GOOD company which is our sending company 
2. BAD GUY an attacker attempting to pretend to be from GOOD company
3. RECEIVING company which is our recipient of the email 

GOOD company has identified their exchange mail server called A as the only mail server that can send mail on the GOOD companies behalf. 
So, when Sara from GOOD sends an email to Dave at RECEIVING, the RECEIVING company verifies this by checking the records at GOOD company. 
Sara's email is originating from mail server A which GOOD company has identified as valid. The email is successfully delivered to Dave at RECEIVING. 

Now, BAD GUY has acquired a mail server that we will call B. BAD GUY attempts to impersonate Sara from GOOD. He sends an email to Dave at RECEIVING pretending to be Sara. 
This time, RECEIVING checks GOOD companies records and determines that the message is originating from mail server B. This is not on GOOD companies authorized mail server 
list. BAD GUY's email is sent to RECEIVING's companies quarantine. 

In our final scenario, GOOD company has acquired a new mail server called C. GOOD forgets to update their SPF records to include mail server C 
as a location that can send mail on behalf of GOOD. As such, Sara from GOOD sends an email to Dave at RECEIVING. The RECEIVING company checks GOOD's authorized mail server 
list for mail server C which is not present. In turn, this legitimate message from Sara at GOOD is sent to Dave's junk/quarantine at RECEIVING. 

## The impact 
Many organizations have a junk email filter that incorporates an SPF check. In turn, if you are a sending organization with incorrect SPF records, then your email 
is likely getting sent to junk/quarantine at multiple organizations in which you communicate. Unfortunately, depending on the organization's reliance on email, this may
cause significant delays in communication resulting in lost profit, time, and energy for all involved. 

## How do I fix my SPF records? 
The first item on my checklist for an email that is suspected of going to quarantine due to improper SPF records is to 
check the email's [message header](https://support.office.com/en-us/article/View-e-mail-message-headers-CD039382-DC6E-4264-AC74-C048563D212C). 

Specifically, I am looking for this line: 

    Received-SPF: Fail (protection.outlook.com: domain of SenderDomain.com does not designate xxx.xx.xxx.xx as permitted sender)

With this knowledge, you can now reach out to the sending organization's IT department to have them update their respective records. 

### SPF Implementation Guides
Excellent! First and foremost, I'm happy that you are interested in updating or creating your SPF records. Please use the following guides/websites/materials at your own risk: 

For Microsoft, check out their PDF [Implementation Tips for the Sender ID Framework - Creating Your SPF Record](https://www.microsoft.com/en-us/download/confirmation.aspx?id=5546)

For Google, check out their web page [Configure SPF records to work with G Suite](https://support.google.com/a/answer/178723?hl=en)

After creating your SPF records or to verify your existing records, you may check syntax via:

- [Kitterman's SPF Record Testing Tools](http://www.kitterman.com/spf/validate.html)
- [Dmarcian SPF Survey](https://dmarcian.com/spf-survey/)

For more information on the breakdown of SPF records, checkout [Ocean's overview](https://www.digitalocean.com/community/tutorials/how-to-use-an-spf-record-to-prevent-spoofing-improve-e-mail-reliability).

## Last minute notes
Maintaining SPF records may be difficult due to the plethora of services that send mail on behalf of an organization's domain.
For example, if the Marketing department decided to use Mail Chimp with the sending address being from their organization, then 
[SPF records](https://blog.mailchimp.com/senderid-authentication-for-your-mailchimp-campaigns/) would need to be updated to allow proper mail flow. 
If the marketing department was unaware of this required change to SPF records and inadvertently forgot to inform IT, then SPF records would be incorrect. 

Furthermore, SPF records can only be modified by the organization that owns the domain (the sending organization). After all, we do not want 
attackers/spammers to update the list of allowed mail servers/services that send mail on behalf of a domain that the attacker does not control. 

Last, email may be successfully delivered at other organizations that do not have a junk email filter or do not perform SPF checks. This can sometimes add to
the confusion or misunderstanding that mail flow issues are on the receiving end rather than the sending end. Often, when I find myself in these types of scenarios, 
I remember to follow the guideline, "Trust; but verify" 

Cheers,

Jeremiah