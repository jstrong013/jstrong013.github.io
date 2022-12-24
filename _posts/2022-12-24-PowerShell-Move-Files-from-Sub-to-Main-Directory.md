---
title: "Use PowerShell to Move Files from sub directory to main directory"
comments: true
tags: PowerShell
excerpt: "Move files from one directory to another. A simple PowerShell script to save time from a manual process."
---
In a previous post, I shared a few PowerShell automation ideas that you could [create today]({{ site.url }}{{ site.baseurl }}{% post_url 2022-11-02-Five-PowerShell-Shortcuts-to-Learn-Today %}?utm_source=InternalBlog&utm_medium=LinkReference&utm_campaign=BlogPost).  
I had concluded in the blog post that any repetitive task is an ideal candidate. 
So today, I found myself performing a task manually that was starting to get cumbersome. 
That is, I was downloading documents then moving them to the root level directory.  

Sure, I could continue to do this process manually and it would only take a few minutes. With that said, a few minutes adds up
over time. After referencing my handy dandy automation calculator *cough*-*cough* err, I mean this [xkcd - Is It Worth The Time?](https://xkcd.com/1205/). 

![Is It Worth the Time?](https://imgs.xkcd.com/comics/is_it_worth_the_time.png)

I realized that I needed to automate this solution. In this post, I'm trying to cover a simple process of creating an automation. 
A PowerShell script that solves my dilemma. The solution isn't beautiful. With that said, it works.  

In my opinion, your goal for automating your repetitive tasks should NOT be beautiful code. Your intent is to accomplish the task you have 
at hand. From here, and as needed, you refractor your script as your experience, knowledge, available time, and needs become greater or clearer.  

## Step 1: Setup Your Problem  
I like to write an abbreviated description of the issue that is being experienced. This can be a simple bullet point list. 

In this scenario, I am downloading files from a course I am taking. They always download into separate folders. I want them to be in the root directory
of the file structure. 

With the basic details out of the way, I write (or illustrate) the desired start and end states. 

My starting state is the following file structure:  
```
Course files
├── Mod1 
│   ├── coursefile1.pdf
│   ├── coursefile2.pdf
├── Mod2 
│   ├── coursefile3.pdf
├── Mod3
│   ├── coursefile4.pdf
└── 
``` 
My desired end state is as follows:  
```
Course files
├── coursefile1.pdf 
├── coursefile2.pdf 
├── coursefile3.pdf
└── coursefile4.pdf
```
By writing this out, I can begin generating assumptions that will help design the solution. The process will bring up concerns to keep in mind.

For example: 
* Will my file names always be unique? In my situation, yes!  
    * Otherwise, I may have to handle duplicated file names    
* Or statements: I am going to get rid of the folders mod1, mod2, and mod3  
    * Do I need to be concerned about data loss?  

You get the gist. A clear description will be the holy grail when you look back in 3 months or 3 years on the background for the analysis 
on the assumptions you made.  

## Step 2: Create the PowerShell Script! 
If you are anything like me, do **NOT** let perfectionism be the blocker to having a solution. An operating script 
can be refactored. An non-existent script does nothing. 

With that said, here is my initial solution, [Move-LabFilesRoot](https://github.com/jstrong013/PublicScripts/blob/c86b737529ef9f78f91338290b3eb8e4f43addb6/Move-LabFilesRoot.ps1#L1-L63):  

```powershell
function Move-LabFilesRoot {
    [CmdletBinding()]
    param (
        # LabFilesPath The root folder of the Course Files.
        [Parameter()]
        [ValidateScript( { Test-Path $PSItem -PathType Container } )]
        [string]
        $LabFilesPath
    )

    if ($LabFilesPath -eq $env:SystemDrive) {
        throw "Do not run this on $($env:SystemDrive)"
    }

    $allFiles = Get-ChildItem -Path $LabFilesPath -Recurse -File

    foreach ($file in $allFiles) {
        Move-Item -Path $file.Fullname -Destination $LabFilesPath -Force
    }

    # Not checking for hidden items 
    $allDirectories = Get-ChildItem -Path $LabFilesPath -Recurse -Directory  

    foreach ($directory in $allDirectories) {
        if ($directory.GetFiles().Count -eq 0) {
            Remove-Item -Path $directory.Fullname -Force 
        }
        else {
            Write-Warning -Message "Did not delete directory: $($directory.Name). Contents may still exist in directory."
        }
    }
}
```  

For the most up-to-date version of [Move-LabFilesRoot](https://github.com/jstrong013/PublicScripts/blob/main/Move-LabFilesRoot.ps1) click [here](https://github.com/jstrong013/PublicScripts/blob/main/Move-LabFilesRoot.ps1). 

### Final Notes 
This is a simple, ineloquent solution to my problem. However, it operates great! So far, it saves me the headache of having to manually do
this via the File Explorer GUI every time. Are there better, more lovely designs. Absolutely! 
With that said, do not make yourself the blocker. Create scripts to 
continue to learn. Heck, if you would like to improve this [script](https://github.com/jstrong013/PublicScripts/blob/main/Move-LabFilesRoot.ps1), feel free to make a pull request on Github!  

What are your monotonous tasks that you plan to automate? Feel free to share below in the comments.  

In any case, I hope you feel inspired to create a solution today.  

Until next time,  
Jeremiah  