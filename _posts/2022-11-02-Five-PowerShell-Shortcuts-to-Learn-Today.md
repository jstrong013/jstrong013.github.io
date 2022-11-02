---
title: "Five PowerShell Shortcuts to Learn Today"
comments: true
tags: [PowerShell,Shortcuts,Tips]
excerpt: "My favorite PowerShell console shortcuts that I needed to learn sooner."
---
Microsoft PowerShell is my favorite scripting language! Hands down, no question.  

Moving around a shell can greatly improve your speed, efficiency, and focus. The following shortcuts are my top *daily* shortcuts.  

# My Five Key Daily Shortcuts  

1. **ESC** key. This one clears the line on your console. If I ever make a mistake, this one keyboard press does the trick. 
2. **Ctrl** + **L**. That is, clear the console screen. The command has the distinct beauty of clearing previous command clutter. You may also just type **Clear** + **Enter** keyboard combo. I suppose, I should give the PowerShell way ```Clear-Host```  
3. I love this one for documentation purposes: **Ctrl** + **Shift** + **C**. This powerful shortcut allows you to copy your current line. I'm crying happy tears ever since I have become aware of this shortcut.  
4. If the last command did not rock your world, then this shortcut certainly will: **Ctrl** + **[Spacebar]**. This provides all available options. 
If using with parameters, it displays all available parameter options. Try this now, type ```Get-History -``` (note the ```-```). Now use the shortcut.   
 ![Ctrl Spacebar Shortcut]({{ site.url }}{{ site.baseurl }}/images/powershell/PowerShellAutoComplete.png)
I like to combine this command with **tab** which does auto-complete.  

5. **Ctrl** + **R** will make searching for that previous command easy! After the shortcut, simply start typing an characters that you believe was 
contained in your command history. The speed and flexibility of this command is not to be passed up. Yes, we could use the above command ```Get-History```.

## Bonus Shortcut  
One of the most common things I do when logging onto a Windows computer is launching PowerShell. My ultimate, mind shattering shortcut for daily use is 
**Windows Key** + **X** + **A**.  This keyboard combo will launch Windows PowerShell Console as an Administrator.  

Fellow PowerShell enthusiast, I hope you were able to learn something new and are able to shave a few milliseconds of time with the use of the above shortcuts.  
Every day I learn something new, so if you have a favorite PowerShell console shortcut then I would love if you shared it in the comments.

Until next time,  
Jeremiah  