---
title: Cloud Voicemail: Transfer the Call Fail - Solved
comments: true
tags: [Skype for Business, Office 365]
excerpt: Upon migrating from Unified Messaging (UM) to Cloud Voicemail (CV), the call answer rule to transfer a call randomly broke.  
---
On November, 2019, our organization decided to migrate from Exchange Unified Messaging Online to Cloud Voicemail.

To do this, we opted into a planned migration ([step 6](https://docs.microsoft.com/en-us/SkypeForBusiness/plan/exchange-unified-messaging-online-migration-support#voicemail-migration-steps)).

Immediately after migrating, voicemail operated as expected. However, Call Answer Rules in [voicemail settings](https://support.office.com/en-us/article/check-skype-for-business-voicemail-and-options-2deea7f8-831f-4e85-a0d4-b34da55945a8?ui=en-US&rs=en-US&ad=US) did not work. More specifically, the call transfer rules did not work:
* Play greeting, then transfer the call
* Play greeting, then allow the caller to recording a message or transfer to the target user

The voicemail greeting would play but when it got time to perform the transfer, the call hung in dead silence. The call would remain connected in this dead air space until you eventually decided to hang up.

## Solved!
Immediately, a case was submitted with Office 365 support. After 3 months and an army of UC experts and network professionals, the issue was discovered to be due to a rule on our Cisco ASA Firewall blocking China. After removing this rule, both voicemail and call answers rules operated as expected.

### More Details
The Microsoft cloud edge server was unable to establish the [MTLS](https://docs.microsoft.com/en-us/skypeforbusiness/plan-your-deployment/security/tls-and-mtls) session with our on-premises edge server.

Since our edge server exists in the United States, it is interesting that the Microsoft Cloud edge server was going to Singapore or Hong Kong (China) which is on the other side of the world.

Upon asking Microsoft why the Asia-Pacific (APAC) Microsoft cloud edge servers were making connections to our on-premise edge server, this was the response:

> There are multiple reasons why routing might end up coming through an APAC edge. ExUM addresses SfBO Forest C edges internally using a single global FQDN that is managed by GTM. There are six geographic sites, two in each NOAM/EMEA/APAC region. GTM might choose to direct traffic through APAC for load balancing, or because a NOAM edge site is down, or because the source Exchange server is in APAC for some reason.

One of the coolest things is [Microsoft Docs being on Github](https://docs.microsoft.com/en-us/contribute/). To hopefully help the next helpdesk individual, I created an [issue](https://github.com/MicrosoftDocs/OfficeDocs-SkypeForBusiness/issues/3935#issue-562091834) to add geo-location consideration on the firewall documentation.

Until next time,  
Jeremiah
