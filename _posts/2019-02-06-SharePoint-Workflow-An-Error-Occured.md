---
title: SharePoint Workflow an Error Occured
comments: true
provider: facebook
tags: [SharePoint]
excerpt: Solving a SharePoint Workflow that reported "Error Occured"
---
Just a little before lunch, I got notified by an employee that something was
wrong with their submitted form. The form in question was tied
to a heavily customized, involved, and very long workflow that includes many steps,
approvals, and actions.

Upon opening up the workflow history, I saw this error message:

    The workflow could not update the item, possibly because one or more columns
    for the item require a different type of information - Outcome: Unknown error

    An error has occurred in <workflow name>

    For more information, please read this article: http://go.microsoft.com/fwlink/?LinkID=323543&clcid=0x409

Of course, I navigated to the page:

> **The workflow could not update the item, possibly because one or more columns
for the item require a different type of information**
> This error commonly results from one of two situations:
> * One of the list fields was removed or changed, but the workflow was not
> updated to account for the change and is therefore trying to set a value for
> the old field. You should check all **Update List Item** actions in your workflow
> and make sure they are setting appropriate values for fields and that those
> fields exist on the list.
> * There is a data type error wherein the workflow is trying to set a value in
> a field in the list item using the wrong data type. You should confirm that
> the **Return Field As** operation in their lookup is of the correct data type.

As far as I could tell, no update items associated in the workflow were failing.
Feeling under pressure to get this business critical process back to an operational
status, I Googled to my hearts content to solve the issue.

Unfortunately, my Google-ing skills yielded zero applicable results. So, I turned
back to the workflow, going through step-by-step.

Upon this deeper investigation, I discovered that the workflow that reports "error occurred"
has a stage that creates 19 additional approval workflows in a separate library. On one
of the nineteen approval tasks, the item fails to create thus causing the failure on the
main workflow in question.

## The Solution
Of the 19 approval workflows, one of the approvers was a departed employee that had their Active Directory (AD) account
disabled. To restore immediate business critical functionality, I simply enabled the AD account of the approver.

After restoring service, the workflow (and associated components) were updated to remove
this account so it could be disabled again.

## The Root cause
In essence, the problem ended up being the account not having permission to update the
approval item in the workflow code due to being disabled in AD. As most tasks in a workflow run under the System
Account and the complexity of this workflow, it took just a bit longer than normal to
discover the reason for this error.

And who says IT is not fun?!

Cheers,

Jeremiah
