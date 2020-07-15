# Mail Functions



Corrupted Attachments ???<br>I spent many hours with corrupted attachments (of all types of files) - The answer: a blank line is needed after $msg.=$file \r\n \r\n [incredible but true].<br>Heres some useful code for sending an attachment, and display html OR text depending on the users email-reader.<br><br>i work with many different systems, so...<br><br>

```
<?php # Is the OS Windows or Mac or Linux
if (strtoupper(substr(PHP_OS,0,3)=='WIN')) {
  $eol="\r\n";
} elseif (strtoupper(substr(PHP_OS,0,3)=='MAC')) {
  $eol="\r"; 
} else {
  $eol="\n"; 
} ?>
```




```
<?php
# File for Attachment
$f_name="../../letters/".$letter;    // use relative path OR ELSE big headaches. $letter is my file for attaching.
$handle=fopen($f_name, 'rb');
$f_contents=fread($handle, filesize($f_name));
$f_contents=chunk_split(base64_encode($f_contents));    //Encode The Data For Transition using base64_encode();
$f_type=filetype($f_name);
fclose($handle);
# To Email Address
$emailaddress="user@example.com";
# Message Subject
$emailsubject="Heres An Email with a PDF".date("Y/m/d H:i:s");
# Message Body
ob_start();
  require("emailbody.php");     // i made a simple &amp; pretty page for showing in the email
$body=ob_get_contents(); ob_end_clean();

# Common Headers
$headers .= 'From: Jonny &lt;jon@example.com&gt;'.$eol;
$headers .= 'Reply-To: Jonny &lt;jon@example.com&gt;'.$eol; 
$headers .= 'Return-Path: Jonny &lt;jon@example.com&gt;'.$eol;     // these two to set reply address
$headers .= "Message-ID:&lt;".$now." TheSystem@".$_SERVER['SERVER_NAME']."&gt;".$eol;
$headers .= "X-Mailer: PHP v".phpversion().$eol;           // These two to help avoid spam-filters
# Boundry for marking the split &amp; Multitype Headers
$mime_boundary=md5(time());
$headers .= 'MIME-Version: 1.0'.$eol; 
$headers .= "Content-Type: multipart/related; boundary=\"".$mime_boundary."\"".$eol; 
$msg = "";

# Attachment
$msg .= "--".$mime_boundary.$eol;
$msg .= "Content-Type: application/pdf; name=\"".$letter."\"".$eol;   // sometimes i have to send MS Word, use 'msword' instead of 'pdf'
$msg .= "Content-Transfer-Encoding: base64".$eol;
$msg .= "Content-Disposition: attachment; filename=\"".$letter."\"".$eol.$eol; // !! This line needs TWO end of lines !! IMPORTANT !!
$msg .= $f_contents.$eol.$eol;
# Setup for text OR html
$msg .= "Content-Type: multipart/alternative".$eol; 

# Text Version
$msg .= "--".$mime_boundary.$eol;
$msg .= "Content-Type: text/plain; charset=iso-8859-1".$eol;
$msg .= "Content-Transfer-Encoding: 8bit".$eol;
$msg .= "This is a multi-part message in MIME format.".$eol;
$msg .= "If you are reading this, please update your email-reading-software.".$eol;
$msg .= "+ + Text Only Email from Genius Jon + +".$eol.$eol;

# HTML Version
$msg .= "--".$mime_boundary.$eol;
$msg .= "Content-Type: text/html; charset=iso-8859-1".$eol;
$msg .= "Content-Transfer-Encoding: 8bit".$eol;
$msg .= $body.$eol.$eol;

# Finished
$msg .= "--".$mime_boundary."--".$eol.$eol;   // finish with two eol's for better security. see Injection.

# SEND THE EMAIL
ini_set(sendmail_from,'from@example.com');  // the INI lines are to force the From Address to be used !
  mail($emailaddress, $emailsubject, $msg, $headers); 
ini_restore(sendmail_from);
?>
```
<br><br><br>I hope this helps.<br>Jon Webb [Madrid&amp;London]<br><br>[EDITOR thiago NOTE: Code has a fix from o.straesser AT gmx DOT de]  

#

As mentioned earlier, for Windows users there is a fake sendmail option. A bit more detailed description how to do this is:<br><br>If you have a test server in use running Windows and some kind of WAMP combo (XXAMP, WAMP Server, etc) then you&apos;ll notice that the PHP sendmail command (mail()) does not work. Windows simply does not provide the sendmail statement ...<br><br>There is a simple trick to get this to work though;<br><br>1) Download (or use the attached file) sendmail.zip from http://glob.com.au/sendmail/<br><br>2) Unzip this in a folder on your c: drive (preferably use a simple path, for example c:\wamp\sendmail -- long filenames could cause problems)<br><br>3) Edit your PHP.INI file (note: WAMP users should access their php.ini file from the WAMP menu). Go to the [mail function] section and modify it as such:<br><br>[mail function]<br>; For Win32 only.<br>;SMTP =<br><br>; For Win32 only.<br>;sendmail_from =<br><br>; For Unix only.  You may supply arguments as well (default: "sendmail -t -i").<br>sendmail_path = "C:\wamp\sendmail\sendmail.exe -t"<br><br>; Force the addition of the specified parameters to be passed as extra parameters<br>; to the sendmail binary. These parameters will always replace the value of<br>; the 5th parameter to mail(), even in safe mode.<br>;mail.force_extra_paramaters =<br><br>.. and save the changes.<br><br>4) Open the sendmail.ini and modify the settings to:<br><br>[sendmail]<br><br>; you must change mail.mydomain.com to your smtp server,<br>; or to IIS&apos;s "pickup" directory.  (generally C:\Inetpub\mailroot\Pickup)<br>; emails delivered via IIS&apos;s pickup directory cause sendmail to<br>; run quicker, but you won&apos;t get error messages back to the calling<br>; application.<br><br>smtp_server=mail.yourdomain.com<br><br>; smtp port (normally 25)<br><br>smtp_port=25<br><br>; the default domain for this server will be read from the registry<br>; this will be appended to email addresses when one isn&apos;t provided<br>; if you want to override the value in the registry, uncomment and modify<br><br>default_domain=yourdomain.com<br><br>; log smtp errors to error.log (defaults to same directory as sendmail.exe)<br>; uncomment to enable logging<br>; error_logfile=sendmail_error.log<br><br>; create debug log as debug.log (defaults to same directory as sendmail.exe)<br>; uncomment to enable debugging<br>; debug_logfile=sendmail_debug.log<br><br>; if your smtp server requires authentication, modify the following two lines<br><br>;auth_username=<br>;auth_password=<br><br>; if your smtp server uses pop3 before smtp authentication, modify the<br>; following three lines<br><br>pop3_server=mail.yourdomain.com<br>pop3_username=you@yourdomain.com<br>pop3_password=mysecretpassword<br><br>; to force the sender to always be the following email address, uncomment and<br>; populate with a valid email address.  this will only affect the "MAIL FROM"<br>; command, it won&apos;t modify the "From: " header of the message content<br><br>force_sender=you@yourdomain.com<br><br>; sendmail will use your hostname and your default_domain in the ehlo/helo<br>; smtp greeting.  you can manually set the ehlo/helo name if required<br><br>hostname=<br><br>The optional error and debug logging is recommended when trying this the first time, so you have a clue what goes wrong in case it doesn&apos;t work.<br>Force_sender is also optional, but recommended to avoid confusion on the server end.<br>Obviously mail.yourdomain.com, you@yourdomain.com, and mysecretpassword should be the relevant info for your SMTP server.<br>Now restart the WAMP services (mainly Apache so PHP re-reads it&apos;s config).<br><br>Now you&apos;re good to go and use the PHP mail() statement as if you&apos;re a Unix user ...  

#

[Official documentation page](https://www.php.net/manual/en/ref.mail.php)

**[To root](/README.md)**