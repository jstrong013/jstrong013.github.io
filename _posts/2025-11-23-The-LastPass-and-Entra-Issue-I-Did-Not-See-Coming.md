---
title: "The LastPass and Entra Issue I Did Not See Coming"
comments: true
tags: [Entra,Software]
categories: [Software]
excerpt: "How a simple SSO setup turned into a mobile access headache."
---
Single Sign-On (SSO) makes enterprise environments more secure and provides end users with a seamless login experience.  

Setting up LastPass SSO with Entra ID is fairly straightforward, and LastPass’s own [guide](https://support.lastpass.com/s/document-item?language=en_US&bundleId=lastpass&topicId=LastPass/Federated_Azure_AD.html&_LANG=enus) is overall quite comprehensive.  

However, there is an important challenge in this setup. If you have an Entra Conditional Access policy scoped to "All resources (formerly All cloud apps)", you cannot exclude the LastPass enterprise application from that policy.  

This behavior is tied to the [`User.ReadWrite`](https://support.lastpass.com/s/document-item?language=en_US&bundleId=lastpass&topicId=LastPass/Federated_Azure_AD_step3_create_login_app.html&_LANG=enus) delegated permission used in the LastPass app configuration.  


While [lower privileged scopes can be bypassed](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-cloud-apps#conditional-access-behavior-when-an-all-resources-policy-has-an-app-exclusion) when an application is excluded from a Conditional Access policy, the higher privileged `User.ReadWrite` scope does not allow for this type of exclusion.  

As a result, if you have a Conditional Access policy configured to Block Access or Grant Access with "Require approved client app" or "Require app protection policy", and you intend to bypass the LastPass enterprise app, the authentication will be blocked. 
The issue becomes more pronounced in environments that require Mobile App Management for all authentications from iOS and Android devices.  

Since the LastPass mobile application is not available as an Intune supported app in either the Apple App Store or the Google Play Store, these logins are effectively blocked.  

## LastPass Support Suggested Workaround  
After raising this concern with LastPass support, the recommended workaround is to avoid using "All resources (formerly All cloud apps)" and instead configure the policy to target specific applications, excluding LastPass by omission.  

If this approach is not acceptable because your organization requires an exclusion based model rather than an inclusion based model due to security posture, administrative overhead, or operational standards, you may need to reconsider implementing LastPass SSO at this time.  

### Final Comments  
If you have run into this issue and found a workaround or a different approach, I would really appreciate hearing how you handled it. 
Feel free to share your experience in the comments so others can learn from it as well.  
