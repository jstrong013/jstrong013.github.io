---
title: Send a MessageMedia Text Message with PowerShell
comments: true
tags: [Software]
excerpt: Send a text with the MessageMedia API using PowerShell.
---
I recently stumbled upon the [MessageMedia Messages API](https://developers.messagemedia.com/). Immediately, I reached out to MessageMedia support to get my REST keys.

As soon as I got the authentication tokens, I was ready to create a quick and dirty script to send a text message with PowerShell to my cell phone.

```PowerShell
  $user = #Gotten from MessageMedia Support
  $pass = #Gotten from MessageMedia Support
  $pair = "$($user):$($pass)"

  $encodedCreds = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($pair))

  $basicAuthValue = "Basic $encodedCreds"

  $Headers = @{
    "Authorization" = $basicAuthValue
  }

  $theMessage = @'
{
    "messages":[
      {
        "content":"I'm excited to send myself this text!",
        "destination_number":"+19999999999",
        "format":"SMS"
      }
    ]
}
'@

  $response = Invoke-WebRequest -Uri 'https://api.messagemedia.com/v1/messages' -Method Post -ContentType "application/json" -Headers $Headers -Body $theMessage

  #Check your Destination Number for your text message!
```
I am a happy recipient of a text message that I sent to myself!

Happy texting friends! I'm back to learning Azure using [Microsoft Learn](https://docs.microsoft.com/en-us/users/JeremiahStrong/).

Cheers,  
Jeremiah
