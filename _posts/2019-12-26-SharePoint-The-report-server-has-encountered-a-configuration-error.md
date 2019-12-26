---
title: SharePoint - The report server has encountered a configuration error
comments: true
tags: [SharePoint]
excerpt: An error occurs when attempting to run a SharePoint report.
---
As I was going through my daily morning ritual of reviewing challenging tickets, I noticed two of particular interest stating that a SharePoint report could not be executed. 

Attempting to run the report myself, I got the error: 

    The report server has encountered a configuration error. Logoin failed for the execution account (rsServerConfigurationError)
        Log on failed. Ensure the user name and password are    correct. (rsLoginFailed)
            For more information about this error navigate to the report server on the local machine, or enable remote errors.

The report in question, used a Microsoft SharePoint List as the data source type. About a week ago, one of my coworkers changed the passwords on all of the SharePoint Service Accounts. 

## The Solution
To fix, I opened Central Administration navigating to the [Execution Account page](https://docs.microsoft.com/en-us/sql/reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application?view=sql-server-2016#execution-account):

    Central Administration > Manage Service Applications > SSRS Service Applications > Execution Account

If you have specified an Execution Account, update the domain account to be appropriate. In my case, the password field.

Upon updating, I ran the report again with success. 

Until next time,  
Jeremiah