---
layout: post
title:  "SMTP Bundle"
date:   2014-12-23
author: "Sean McLellan"
tags: ["SMTP bundle"]
---

Occasionally, we might find the need to send an Email message that contains one or more attachments.

Although SharePoint has the "SendEmail" function on SPUtility (and exposed currently through Barista) this method doesn't provide the ability to add attachments to the outgoing mail message... thus forcing the developer to go bonkers:
http://edwin.vriethoff.net/2007/10/02/how-to-send-an-e-mail-with-attachment-from-sharepoint/



With the Smtp bundle, we can write the following code to perform similar functionality:

```javascript
require("Smtp");
var sp = require("SharePoint");

//Grab the outgoing smtp server configured in CA for the current web application.
var outgoingSmtpServer = sp.currentContext.site.getWebApplication().outboundMailServiceInstance

//Create a new SmtpClient with the outgoing server address
var client = new SmtpClient(outgoingSmtpServer.server.address);

//Load an existing file from SharePoint
var file = sp.loadFileAsByteArray("/Documents/BreakReport.xlsx");

//Create a new message and add the file we loaded as an attachment.
var message = new MailMessage("sean.mclellan@sixconcepts.com", "somesharepointguy@ofsportal", "Hi", "testtesttesttest");
message.attachments.add(new SmtpAttachment(file));

//Add an alternate view for mail readers that understand html.
message.alternateViews.add(AlternateView.createAlternateViewFromString("<html><body><b>Hi!</b></body></html>", "Unicode", "text/html"));
                           

//Send the mail message synchronously.
client.sendMailMessage(message);
```

results: 
![alt text](/img/2014-12-23-smtp-bundle-01.png "Email Results")

'till next time.