---
title: "CaseWare Cloud: the Good, the Bad, and the Meh"
comments: true
tags: [Software, Opinion]
excerpt: Thinking of moving your on-premises CaseWare Working Papers Solution to CaseWare Cloud? Read this review from an Administrator and management perspective.
---
[CaseWare Cloud](https://www.casewarecloud.com/) is CaseWare International's Software as a Service offering for tracking an engagement, collaborating on work in progress, and storage of client files.

The solution is enticing for any accounting firm that is looking to stay modern. With that said,
a few drawbacks exist. I hope to go over both in this quick and simple post for the
concerns of a System/Application Administrator who is using CaseWare Working Papers with an on-premises file server solution at this time but is considering moving to the CaseWare Cloud solution.

With that said, I always appreciate posts that give the bottom line upfront. So, without further ado, CaseWare Cloud gets my vote of approval to move from your file server to CaseWare's infrastructure.

## The Good
* You can work from anywhere as long as you have a workstation with CaseWare Working Papers installed.
  * No VPN or Citrix required when out of office to access and update client files
  * CaseWare SmartSync copies for large files can be taken out at client site with likely better performance than your VPN solution
  * Two-Factor authentication for text to cellular may be enabled on the tenant
* No more server maintenance tasks: Windows Updates and hardware upgrades cease to exist
* Purchasing additional User licensing via the [MyCase CaseWare](https://my.caseware.com/account/login) Portal is easy and quick
* Security ([staff roles](https://docs.caseware.com/2019/webapps/30/en/Setup/Security/Built-in-roles.htm)) is intuitive and easy for the system level of access.
  * In other words, if you have a single folder that contains all of your CaseWare files with NTFS permissions that inherit on those CaseWare files this will be an easy cutover for you.
* CaseWare Cloud is "compliant with all major security control frameworks" ([website source](https://www.caseware.com/solutions/cloud/security))

## The Bad
The negatives of the CaseWare Cloud solution only apply to medium to large size firms or firms that follow enterprise level standards.
* CaseWare Cloud is a Sync only environment. In turn, this means a lot of client data lives on workstations. No easy methodology or a premade report exists to show which client files have sync copies.
  * On an aside, it is easy to see a list of sync copies on a single Working Papers file from CaseWare Cloud interface.
* Two-Factor authentication for number management may only be managed by the User making for tougher troubleshooting
  * The staff profile field *Mobile Phone* is not their two-factor number
* No integration with your Active Directory infrastructure for single sign-on
* No Active Directory synchronization for User creation/removal
* No integration to your backup software - You have to rely on CaseWare's backup strategy:
  * A [recycle bin](https://docs.caseware.com/2019/webapps/30/en/Practice/Maintenance/Recover-a-previous-version-or-deleted-file.htm) exists for end users to recover deleted files
  * If not in the recycle bin and up to a maximum of 90 days, a paid recovery request can be made to support. After 90 days, the data is not recoverable.
  * Recovery takes a minimum of 24 hours

## The Meh
* The security model (staff roles) is perfect for a single office. If you are a multi-office or multi-department/service line organization where the system level is to much access then access control will need to be applied to the Entity
  * If you use AD Groups for access control on subfolders then this model is tough to implement in a manageable, audit reporting way
* No API. Since Active Directory synchronization is not an option, it would be nice to see an API option

### Final Thoughts
I believe *the bad* items are solvable. As larger firms adopt CaseWare Cloud or engineering focuses efforts on those categories, the CaseWare Cloud solution will make Administrators encourage the option without question. Well, at least I would. 
