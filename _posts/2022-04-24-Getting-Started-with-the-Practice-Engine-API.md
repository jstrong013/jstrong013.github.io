---
title: "Getting Started with the Practice Engine API"
comments: true
tags: [Software,PowerShell]
excerpt: "A field guide to get underway with the PE API."
---
The [IRIS Practice Engine (PE)](https://www.irisglobal.com/products/accounting-practice-management/iris-practice-engine/) platform offers an API to create robust, custom solutions.
Or, in my case, a method to automate receiving an email for unreleased time.  
As my preferred scripting language of choice, I used PowerShell.

**INFO:** The following overview has an assumption that you have a familiarity with APIs. As such, the
information may be incomplete due to this assumption.  
{: .notice--primary}  

# Before You Begin  
Before you can start, create an account to perform your API queries. The Staff Account needs
permissions for any query that you are aiming to do. To begin and for testing, consider
a full Admin account.  

After Staff Account creation, go to the Admin > Task Pad > API Authentication Administration. Choose 'Create New App Id' and
input the User Id of the User you created above. Then hit save. It will look like this:  

![Administer API Access]({{ site.url }}{{ site.baseurl }}/images/practiceengine/administerapiaccess-apiauthenticationcreate.png)  

**Reminder** Be sure to hit save!  
{: .notice--success}  

## Authorization Header
In order to make a query, generate an authentication token. The PE API uses bearer tokens.  

The token endpoint URL: ```https://contoso.pehosted.com/auth/connect/token```  
Be sure to change ```contoso``` for your site.

PE API version 9.6 and later uses the OpenID connect protocol.  

### PowerShell
To create the authorization header in PowerShell

<script src="https://raw.githubusercontent.com/jstrong013/PracticeEngine/7cf3ca68528f183ad64176577f14cdc5a3ae505b/Create-PEAuthorizationHeader.ps1"></script>

### Power Query M
For the [Power Query M formula language](https://docs.microsoft.com/en-us/powerquery-m/)

```
let
    app_id = "<appid>",
    app_key = "<appkey>",

    serverurl = "https:/contoso.pehosted.com",

    token_uri = serverurl & "/auth/connect/token",

     BasicAuth = "Basic " & Binary.ToText(Text.ToBinary(app_id & ":" & app_key),0),

     GetTokenRequestJson = Web.Contents(token_uri,
        [
            Headers = [#"Authorization"=BasicAuth,
                       #"Content-Type"="application/x-www-form-urlencoded"
            ],
            Content = Text.ToBinary(Uri.BuildQueryString(
                [
                    grant_type = "client_credentials",
                    scope = "pe.api"
                ]
            ))
        ]
    ),

    FormatTokenRequestJson = Json.Document(GetTokenRequestJson),
    AccessToken = FormatTokenRequestJson[access_token],

    BearerTokenHeader = "Bearer " & AccessToken
in
    BearerTokenHeader

```
### Python
Github User [KennyKennedy](https://github.com/kennykennedy) can help us here.

```NumPy
import requests, json

servurl = 'https://contoso.pehosted.com'
appid = '<appid>'
appkey = '<appkey>'

tokenurl = servurl+'/auth/connect/token'

auth = (appid,appkey)
authtype = {'grant_type': 'client_credentials', 'scope': 'pe.api'}
resptoken = requests.post(tokenurl, data=authtype, auth=auth)
token = resptoken.json()['access_token']
apiheader = {'Authorization': 'Bearer ' + token}

```
### Postman
Launch the Postman client. Create a new HTTP request with the POST command.
The request URL: https://contoso.pehosted.com/auth/connect/token  
Create three keys with the following values:

| KEY          | VALUE       |
| -----------  | ----------- |
| grant_type   | client_credentials   |
| client_id    | *app id*     |
| client_secret| *app key*    |

It will look something like this:  

![Bearer token creation via Postman]({{ site.url }}{{ site.baseurl }}/images/practiceengine/pe-postman-bearertokenrequest.png)  

The returned value, access_token, is for the authorization Headers in your API requests. It will be in format 'Bearer *access_token*'

The Headers section will have the key/value entry:

| KEY          | VALUE       |
| -----------  | ----------- |
| Authorization   | Bearer access_token   |

And will look like the following:  

![Postman header value]({{ site.url }}{{ site.baseurl }}/images/practiceengine/pe-postman-headervalue.png)   

## Your First Query!
To get underway, the easiest example is to get details about the staff account you created.  

This can be done from the authorization headers created in the [previous section](#authorization-header).

Remember to update ```contoso``` for your site.

### PowerShell  

```PowerShell
$me = Invoke-RestMethod -Uri 'https://contoso.pehosted.com/pe/api/StaffMember/Me' -Method Get -Headers $authorizationheader
```

### Power Query M

```
let
  peme_uri = "https://contoso.pehosted.com/PE/api/StaffMember/Me",
  GetPEJsonQuery = Web.Contents(peme_uri,
        [
            Headers = [#"Authorization"=BearerTokenHeader]
        ]
    ),
    FormatPEJsonQuery = Json.Document(GetPEJsonQuery)

in
    FormatPeJsonQuery
```

### Python

```NumPy
from pprint import pprint
apiurl = 'https://contoso.pehosted.com/pe/api/StaffMember/Me'

respapi = requests.get(apiurl, headers=apiheader)
pprint(respapi.json())

```

### Postman  
The request URL: https://contoso.pehosted.com/pe/api/StaffMember/ME  

![Postman API query]({{ site.url }}{{ site.baseurl }}/images/practiceengine/pe-postman-api-staffmemberme.png)    


## The Second Query  
To explore the vast number of queries you can do, you could checkout [swagger](#helpful-resources-referenced).

With that said, I find it easiest and best to use a packet capture program while doing the actual actions in the PE site.
I prefer [Wireshark](https://www.wireshark.org/download.html) or [Google Chrome DevTools](https://support.google.com/admanager/answer/10358597?hl=en).
The best part about using this option is your query and response are captured. As such, you will know exactly the ideal
payloads to use.  

## Helpful Resources Referenced  
The Practice Engine API has a lot of helpful resources.  

**INFO:** The Swagger documentation and API Help Page may take a moment to load as it builds dynamically. Remember to replace **Contoso** for your site.  
{: .notice}  

* [Github PracticeEngine](https://github.com/PracticeEngine)
  * [API Examples](https://github.com/PracticeEngine/ApiExamples)  
  * [Postman Collection](https://github.com/PracticeEngine/ApiHelp/blob/master/Practice%20Engine%20API.postman_collection.json)  
* Swagger: https://contoso.pehosted.com/PE/swagger  
* API Help Page: https://contoso.pehosted.com/PE/ApiHelp  

At the time of this writing, the PE Github does not appear to accept [Pull Requests](https://github.com/PracticeEngine/ApiExamples/pull/2)
so I did create a [fork](https://github.com/jstrong013/ApiExamples).  
In a new repository, I added the following examples that are in this post:  
* [PowerShell](https://github.com/jstrong013/PracticeEngine/blob/master/Get-PEMe.ps1)
* [Power Query](https://github.com/jstrong013/PracticeEngine/blob/master/PowerQueryAPIExample.pq)
* [Python ](https://github.com/jstrong013/PracticeEngine/blob/master/peapi_openid.py) - Thanks [KennyKennedy](https://github.com/kennykennedy)  

#### Closing out

I hope this was helpful! I love automation. My Internal Accounting team appreciates the PE API. After all, it is the PE API that allows me to get email/text reminders for unreleased time in the format that I desire. To be honest, my workload would not be sustainable
without APIs.  

If you have public blog, Github, or code with your PE automations, please drop a comment. I enjoy seeing
others work of art (err, I mean automations).  

Cheers,  
Jeremiah
