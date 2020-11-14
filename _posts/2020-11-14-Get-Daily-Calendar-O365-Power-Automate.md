---
title: "Get Your Daily Calendar Events from your Office 365 Calendar with Power Automate"
comments: true
tags: [Software]
excerpt: "Create a Microsoft Flow to send you an email with your daily calendar agenda."  
---
As a PowerShell lover, I sometimes have a hard time considering different
tools for an automation job. Indeed, a diverse automation
landscape with multiple languages and programs are available to do any task at hand.

So today, I decided to focus on Power Automate (formally called Flow) as my automation platform.

**NOTE:** Throughout this article I will use Power Automate and Flow interchangeably.
{: .notice}

Today's idea comes from this [User Voice](https://office365.uservoice.com/forums/273493-office-365-admin/suggestions/13377009-email-daily-o365-agenda-each-morning).

> Email daily O365 agenda each morning
>> Our company just switched from Gmail to O365. One of the features that I really liked with the Google calendar, was that it emailed me my daily agenda at 5:00 a.m. each day. It would be great if there was a way to do this with the O365 calendar.

While I certainly could create a PowerShell function to complete
this task, I'm going to focus on using [Power Automate](https://docs.microsoft.com/en-us/power-automate/) to
solve this scenario.  

In this post, we will be getting our daily calendar agenda via email at 7:15 AM, Monday
through Friday. The daily agenda will include all events that are occurring on that day
over the next 10 hours. In other words, events happening until 5:15 PM on that day.

Let's get started.

1. Login into [Power Automate](https://us.flow.microsoft.com/)
2. Click on 'My flows'
3. Click New dropdown > choose "Scheduled - from blank"  

![Power Automate Template Choice]({{ site.url }}{{ site.baseurl }}/images/powerautomate/NewFlowFormBlank.png)  

{:start="4"}
4. Enter a Flow name, Starting date, time and desired schedule.

In my flow, I decided to name my flow "My Daily Agenda Flow" with today as the starting date to run at 7:15 AM, Monday through Friday.

![Power Automate Schedule Options]({{ site.url }}{{ site.baseurl }}/images/powerautomate/ScheduleofFlow.png)  

{:start="5"}
5. You will enter the Power Automate editor. Choose "+ New step"
6. For the action, search for "Get Calendar View of events V3" in Actions  
![Action Get Calendar View of Events V3]({{ site.url }}{{ site.baseurl }}/images/powerautomate/ActionGetCalendarViewofEventsV3.png)  
{:start="7"}
7. Enter our desired parameters  

Choose your desired Calendar. In my case, the default calendar.  
For Start Time, we are going to use an Expression: `utcNow()`  
For end time, we are going to use another Expression to get the next 10 hours of events: `addHours(utcNow(),10)`

Select the item menu "Show advanced options"

In the Order By: `Start/DateTime Asc`

It will look similar to this:  
![Get Calendar View of Events V3 Parameters]({{ site.url }}{{ site.baseurl }}/images/powerautomate/GetCalendarViewofEventsParameters.png)  

{:start="8"}
8. Add another step. Choose Action "Create HTML Table"  

Select the item menu "Show advanced options"  
From the 'Value' of the Get Calendar Events  
For headers, make the following: Meeting, Location, Start, and End  
The value of Meeting is Subject from previous action  
The value of Location is Location  
For Start, an expression will be used: `convertTimeZone(item()?['Start'],'UTC','Central Standard Time','t')`  
For End, another Expression will be used: `convertTimeZone(item()?['End'],'UTC','Central Standard Time','t')`  

Your window should look similar to the following:
![Create HTML Table Parameters]({{ site.url }}{{ site.baseurl }}/images/powerautomate/CreateHTMLTable.png)  

**Note:** I'm in the Central Standard Time Zone. If you exist in another
region, you can obtain get your time zone [here](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms912391(v=winembedded.11)).
{: .notice--info}

{:start="9"}
9. Create your final step. The action is 'Send an email'

In the To: field, put your email address.  
Put in your desired subject line. I decided on 'Events for Today'  
The Body is the Output of our previous data operation action.  
Again, choose "Show advanced options" and verify is HTML is set to yes.

Your step will look like this:
![Send an Email Parameters]({{ site.url }}{{ site.baseurl }}/images/powerautomate/SendanEmail.png)  

{:start="10"}
10. If you have not done so, take a moment and SAVE your new Power Automate! Now,
in the upper right-hand corner choose "Test". Select "I'll perform the trigger action" then Test.
Finally, choose Run flow. Check your Inbox for your daily events email!  

From here on out, you will be getting your daily calendar agenda at 7:15AM Monday through Friday.
At this stage, the flow is pretty simple. I encourage you to customize it to fit
your needs.  

I'm off to help assist my coworkers automate our most time consuming tasks.

Have a wonderful time automating!  
