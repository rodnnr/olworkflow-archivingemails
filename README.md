## How to Archive Emails with PReS or PlanetPress Connect using SMTP Input

### Introduction

Users often contact Support to enquire about ways to backup emails sent to their clients for accounting and tracking purposes. Emails can be archived in Workflow v8.6.1+ using the SMTP Input Server. This implies that:
•	The OL Connect Workflow SMTP Server service (or SMTP Server as listed in the Workflow Service Console) is started.
•	Port 25 used by the local SMTP Server is open
•	For archiving purposes, emails are sent using the localhost SMTP Server
### Send Email Process Considerations

The first “Send Emails” process in this example simply send emails using the localhost (or 127.0.0.1) as the SMTP server. Emails sent this way can then be intercepted by the SMTP Input task for further processing.
### Get EML Process

The “Get EML” process uses the SMTP Input plugin to read the incoming SMTP request and provides the data within its body. The SMTP Input plugin can also act as an SMTP Proxy, processing, enhancing, or archiving emails before they are sent to another SMTP Server. 
The SMTP Helper creates a minimum of three files with each emails received:
•	xxxxxx-raw.eml: the actual email dump. This the file we are looking to archive in this example
•	xxxxxx-body.txt: the contents of the email body. 
•	xxxxxx.xml: an XML envelope that contains information about the email

This example only focuses on the archiving of emails. In this case, the SMTP Input data location is set to its Envelope. The received request file, in XML format, includes all email fields such as “ppemail”, “from”, “to”, “bcc”, “subject”, “body”, etc.

    enter code here
    <?xml version="1.0" encoding="UTF-8"?>
    <ppemail rawemail="pps00ZXT5UU54VVY06FB2E9BC-raw.eml" timestamp="22/05/2017 13:25:10">
     <from name="info@mailserver.com" address="info@mailserver.com"/>
     <to>braund@emailserver.com</to>
     <cc></cc>
     <bcc></bcc>
     <subject>Take action</subject>
     <body/>
     <attachments/>
     <header>Received=from serverName[127.0.0.1] (helo=oluk1-noubissr4) by serverName[127.0.0.1] with esmtp (OL Connect Workflow SMTP Server)
     Date=Mon, 22 May 2017 13:25:10 +0100 (BST)
     From=&lt;info@mailserver.com&gt;
     To=braund@emailserver.com
     Message-ID=&lt;525950554.95.1495455910644.JavaMail.username@serverName&gt;
     Subject=Take action
     MIME-Version=1.0
     Content-Type=text/html; charset="UTF-8"
     Content-Transfer-Encoding=quoted-printable
     </header>
     </ppemail>


The request file does not include the full path for each of the file above-mentioned files; however, they can be retrieved using the **_%t%O_** variable to get their current temporary folder.

As such to retrieve the EML file, the **_Load External File_** plugin can be used with the value of:

    %t%O\xmlget('/ppemail[1]/@rawemail',Value,KeepCase,NoTrim)

The output of this **_self-replicating_** process is the fully formatted xxxx.eml files, which can be open and viewed in Microsoft Outlook.

### Notes 
Applies to Connect v1.8+

