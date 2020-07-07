# imap_append





Here is how I used this function with an attachment. I used examples from everyone else but made it easier and cleaner to change.





```
<?php

$authhost=&quot;{000.000.000.000:993/validate-cert/ssl}Drafts&quot;;



$user=&quot;sadasd&quot;;

$pass=&quot;sadasd&quot;;



if ($mbox=imap_open( $authhost, $user, $pass))

{

&#xA0; &#xA0; $dmy=date(&quot;d-M-Y H:i:s&quot;);

&#xA0; &#xA0; 

&#xA0; &#xA0; $filename=&quot;filename.pdf&quot;;

&#xA0; &#xA0; $attachment = chunk_split(base64_encode($filestring));

&#xA0; &#xA0; 

&#xA0; &#xA0; $boundary = &quot;------=&quot;.md5(uniqid(rand()));

&#xA0; &#xA0; 

&#xA0; &#xA0; $msg = (&quot;From: Somebody\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;To: test@example.co.uk\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Date: $dmy\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Subject: This is the subject\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;MIME-Version: 1.0\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Content-Type: multipart/mixed; boundary=\&quot;$boundary\&quot;\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;\r\n\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;--$boundary\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Content-Type: text/html;\r\n\tcharset=\&quot;ISO-8859-1\&quot;\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Content-Transfer-Encoding: 8bit \r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;\r\n\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Hello this is a test\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;\r\n\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;--$boundary\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Content-Transfer-Encoding: base64\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;Content-Disposition: attachment; filename=\&quot;$filename\&quot;\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;\r\n&quot; . $attachment . &quot;\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;\r\n\r\n\r\n&quot;

&#xA0; &#xA0; &#xA0; &#xA0; . &quot;--$boundary--\r\n\r\n&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; imap_append($mbox,$authhost,$msg, &quot;\\Draft&quot;); 



&#xA0; &#xA0; imap_close($mbox);

}

else

{

&#xA0; &#xA0; echo &quot;&lt;h1&gt;FAIL!&lt;/h1&gt;\n&quot;;

}



?>
```




Hope this helps someone.

  

#



Hi,

As we have been struggling with this for some time I wanted to share how we got imap_append working properly with all MIME parts including attachments.&#xA0; If you are sending email and also wish to append the sent message to the Sent Items folder, I cannot think of an easier way to do this, as follows:

1) Use SwiftMailer to send the message via PHP.
$message = Swift_Message::newInstance(&quot;Subject goes here&quot;);
(then add from, to, body, attachments etc)
$result = $mailer-&gt;send($message);

2) When you construct the message in step 1) above save it to a variable as follows:

$msg = $message-&gt;toString(); (this creates the full MIME message required for imap_append()!!&#xA0; After this you can call imap_append like this:

imap_append($imap_conn,$mail_box,$msg.&quot;\r\n&quot;,&quot;\\Seen&quot;);

I hope this helps the readers, and prevents saves people from doing what we started doing - hand crafting the MIME messages :-0

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-append.php)

**[To root](/README.md)**