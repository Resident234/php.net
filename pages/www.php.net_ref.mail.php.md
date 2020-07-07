# Mail Functions





Corrupted Attachments ???

I spent many hours with corrupted attachments (of all types of files) - The answer: a blank line is needed after $msg.=$file \r\n \r\n [incredible but true].

Heres some useful code for sending an attachment, and display html OR text depending on the users email-reader.



i work with many different systems, so...





```
<?php # Is the OS Windows or Mac or Linux

if (strtoupper(substr(PHP_OS,0,3)==&apos;WIN&apos;)) {

&#xA0; $eol=&quot;\r\n&quot;;

} elseif (strtoupper(substr(PHP_OS,0,3)==&apos;MAC&apos;)) {

&#xA0; $eol=&quot;\r&quot;; 

} else {

&#xA0; $eol=&quot;\n&quot;; 

} ?>
```






```
<?php

# File for Attachment

$f_name=&quot;../../letters/&quot;.$letter;&#xA0; &#xA0; // use relative path OR ELSE big headaches. $letter is my file for attaching.

$handle=fopen($f_name, &apos;rb&apos;);

$f_contents=fread($handle, filesize($f_name));

$f_contents=chunk_split(base64_encode($f_contents));&#xA0; &#xA0; //Encode The Data For Transition using base64_encode();

$f_type=filetype($f_name);

fclose($handle);

# To Email Address

$emailaddress=&quot;user@example.com&quot;;

# Message Subject

$emailsubject=&quot;Heres An Email with a PDF&quot;.date(&quot;Y/m/d H:i:s&quot;);

# Message Body

ob_start();

&#xA0; require(&quot;emailbody.php&quot;);&#xA0; &#xA0;&#xA0; // i made a simple &amp; pretty page for showing in the email

$body=ob_get_contents(); ob_end_clean();



# Common Headers

$headers .= &apos;From: Jonny &lt;jon@example.com&gt;&apos;.$eol;

$headers .= &apos;Reply-To: Jonny &lt;jon@example.com&gt;&apos;.$eol; 

$headers .= &apos;Return-Path: Jonny &lt;jon@example.com&gt;&apos;.$eol;&#xA0; &#xA0;&#xA0; // these two to set reply address

$headers .= &quot;Message-ID:&lt;&quot;.$now.&quot; TheSystem@&quot;.$_SERVER[&apos;SERVER_NAME&apos;].&quot;&gt;&quot;.$eol;

$headers .= &quot;X-Mailer: PHP v&quot;.phpversion().$eol;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // These two to help avoid spam-filters

# Boundry for marking the split &amp; Multitype Headers

$mime_boundary=md5(time());

$headers .= &apos;MIME-Version: 1.0&apos;.$eol; 

$headers .= &quot;Content-Type: multipart/related; boundary=\&quot;&quot;.$mime_boundary.&quot;\&quot;&quot;.$eol; 

$msg = &quot;&quot;;



# Attachment

$msg .= &quot;--&quot;.$mime_boundary.$eol;

$msg .= &quot;Content-Type: application/pdf; name=\&quot;&quot;.$letter.&quot;\&quot;&quot;.$eol;&#xA0;&#xA0; // sometimes i have to send MS Word, use &apos;msword&apos; instead of &apos;pdf&apos;

$msg .= &quot;Content-Transfer-Encoding: base64&quot;.$eol;

$msg .= &quot;Content-Disposition: attachment; filename=\&quot;&quot;.$letter.&quot;\&quot;&quot;.$eol.$eol; // !! This line needs TWO end of lines !! IMPORTANT !!

$msg .= $f_contents.$eol.$eol;

# Setup for text OR html

$msg .= &quot;Content-Type: multipart/alternative&quot;.$eol; 



# Text Version

$msg .= &quot;--&quot;.$mime_boundary.$eol;

$msg .= &quot;Content-Type: text/plain; charset=iso-8859-1&quot;.$eol;

$msg .= &quot;Content-Transfer-Encoding: 8bit&quot;.$eol;

$msg .= &quot;This is a multi-part message in MIME format.&quot;.$eol;

$msg .= &quot;If you are reading this, please update your email-reading-software.&quot;.$eol;

$msg .= &quot;+ + Text Only Email from Genius Jon + +&quot;.$eol.$eol;



# HTML Version

$msg .= &quot;--&quot;.$mime_boundary.$eol;

$msg .= &quot;Content-Type: text/html; charset=iso-8859-1&quot;.$eol;

$msg .= &quot;Content-Transfer-Encoding: 8bit&quot;.$eol;

$msg .= $body.$eol.$eol;



# Finished

$msg .= &quot;--&quot;.$mime_boundary.&quot;--&quot;.$eol.$eol;&#xA0;&#xA0; // finish with two eol&apos;s for better security. see Injection.



# SEND THE EMAIL

ini_set(sendmail_from,&apos;from@example.com&apos;);&#xA0; // the INI lines are to force the From Address to be used !

&#xA0; mail($emailaddress, $emailsubject, $msg, $headers); 

ini_restore(sendmail_from);

?>
```






I hope this helps.

Jon Webb [Madrid&amp;London]



[EDITOR thiago NOTE: Code has a fix from o.straesser AT gmx DOT de]

  

#



As mentioned earlier, for Windows users there is a fake sendmail option. A bit more detailed description how to do this is:

If you have a test server in use running Windows and some kind of WAMP combo (XXAMP, WAMP Server, etc) then you&apos;ll notice that the PHP sendmail command (mail()) does not work. Windows simply does not provide the sendmail statement ...

There is a simple trick to get this to work though;

1) Download (or use the attached file) sendmail.zip from http://glob.com.au/sendmail/

2) Unzip this in a folder on your c: drive (preferably use a simple path, for example c:\wamp\sendmail -- long filenames could cause problems)

3) Edit your PHP.INI file (note: WAMP users should access their php.ini file from the WAMP menu). Go to the [mail function] section and modify it as such:

[mail function]
; For Win32 only.
;SMTP =

; For Win32 only.
;sendmail_from =

; For Unix only.&#xA0; You may supply arguments as well (default: &quot;sendmail -t -i&quot;).
sendmail_path = &quot;C:\wamp\sendmail\sendmail.exe -t&quot;

; Force the addition of the specified parameters to be passed as extra parameters
; to the sendmail binary. These parameters will always replace the value of
; the 5th parameter to mail(), even in safe mode.
;mail.force_extra_paramaters =

.. and save the changes.

4) Open the sendmail.ini and modify the settings to:

[sendmail]

; you must change mail.mydomain.com to your smtp server,
; or to IIS&apos;s &quot;pickup&quot; directory.&#xA0; (generally C:\Inetpub\mailroot\Pickup)
; emails delivered via IIS&apos;s pickup directory cause sendmail to
; run quicker, but you won&apos;t get error messages back to the calling
; application.

smtp_server=mail.yourdomain.com

; smtp port (normally 25)

smtp_port=25

; the default domain for this server will be read from the registry
; this will be appended to email addresses when one isn&apos;t provided
; if you want to override the value in the registry, uncomment and modify

default_domain=yourdomain.com

; log smtp errors to error.log (defaults to same directory as sendmail.exe)
; uncomment to enable logging
; error_logfile=sendmail_error.log

; create debug log as debug.log (defaults to same directory as sendmail.exe)
; uncomment to enable debugging
; debug_logfile=sendmail_debug.log

; if your smtp server requires authentication, modify the following two lines

;auth_username=
;auth_password=

; if your smtp server uses pop3 before smtp authentication, modify the
; following three lines

pop3_server=mail.yourdomain.com
pop3_username=you@yourdomain.com
pop3_password=mysecretpassword

; to force the sender to always be the following email address, uncomment and
; populate with a valid email address.&#xA0; this will only affect the &quot;MAIL FROM&quot;
; command, it won&apos;t modify the &quot;From: &quot; header of the message content

force_sender=you@yourdomain.com

; sendmail will use your hostname and your default_domain in the ehlo/helo
; smtp greeting.&#xA0; you can manually set the ehlo/helo name if required

hostname=

The optional error and debug logging is recommended when trying this the first time, so you have a clue what goes wrong in case it doesn&apos;t work.
Force_sender is also optional, but recommended to avoid confusion on the server end.
Obviously mail.yourdomain.com, you@yourdomain.com, and mysecretpassword should be the relevant info for your SMTP server.
Now restart the WAMP services (mainly Apache so PHP re-reads it&apos;s config).

Now you&apos;re good to go and use the PHP mail() statement as if you&apos;re a Unix user ...

  

#

[Official documentation page](https://www.php.net/manual/en/ref.mail.php)

**[To root](/README.md)**