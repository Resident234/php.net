# imap_append



Here is how I used this function with an attachment. I used examples from everyone else but made it easier and cleaner to change.<br><br>

```
<?php
$authhost="{000.000.000.000:993/validate-cert/ssl}Drafts";

$user="sadasd";
$pass="sadasd";

if ($mbox=imap_open( $authhost, $user, $pass))
{
    $dmy=date("d-M-Y H:i:s");
    
    $filename="filename.pdf";
    $attachment = chunk_split(base64_encode($filestring));
    
    $boundary = "------=".md5(uniqid(rand()));
    
    $msg = ("From: Somebody\r\n"
        . "To: test@example.co.uk\r\n"
        . "Date: $dmy\r\n"
        . "Subject: This is the subject\r\n"
        . "MIME-Version: 1.0\r\n"
        . "Content-Type: multipart/mixed; boundary=\"$boundary\"\r\n"
        . "\r\n\r\n"
        . "--$boundary\r\n"
        . "Content-Type: text/html;\r\n\tcharset=\"ISO-8859-1\"\r\n"
        . "Content-Transfer-Encoding: 8bit \r\n"
        . "\r\n\r\n"
        . "Hello this is a test\r\n"
        . "\r\n\r\n"
        . "--$boundary\r\n"
        . "Content-Transfer-Encoding: base64\r\n"
        . "Content-Disposition: attachment; filename=\"$filename\"\r\n"
        . "\r\n" . $attachment . "\r\n"
        . "\r\n\r\n\r\n"
        . "--$boundary--\r\n\r\n");
        
    imap_append($mbox,$authhost,$msg, "\\Draft"); 

    imap_close($mbox);
}
else
{
    echo "&lt;h1&gt;FAIL!&lt;/h1&gt;\n";
}

?>
```
<br><br>Hope this helps someone.  

#

Hi,<br><br>As we have been struggling with this for some time I wanted to share how we got imap_append working properly with all MIME parts including attachments.  If you are sending email and also wish to append the sent message to the Sent Items folder, I cannot think of an easier way to do this, as follows:<br><br>1) Use SwiftMailer to send the message via PHP.<br>$message = Swift_Message::newInstance("Subject goes here");<br>(then add from, to, body, attachments etc)<br>$result = $mailer-&gt;send($message);<br><br>2) When you construct the message in step 1) above save it to a variable as follows:<br><br>$msg = $message-&gt;toString(); (this creates the full MIME message required for imap_append()!!  After this you can call imap_append like this:<br><br>imap_append($imap_conn,$mail_box,$msg."\r\n","\\Seen");<br><br>I hope this helps the readers, and prevents saves people from doing what we started doing - hand crafting the MIME messages :-0  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-append.php)

**[To root](/README.md)**