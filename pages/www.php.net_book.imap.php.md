# IMAP, POP3 and NNTP





For all the people coming here praying for:



1) a dead-easy way to read MIME attachments, or

2) a dead-easy way to access POP3 folders



Look no further.





```
<?php 

function pop3_login($host,$port,$user,$pass,$folder=&quot;INBOX&quot;,$ssl=false)

{

&#xA0; &#xA0; $ssl=($ssl==false)?&quot;/novalidate-cert&quot;:&quot;&quot;;

&#xA0; &#xA0; return (imap_open(&quot;{&quot;.&quot;$host:$port/pop3$ssl&quot;.&quot;}$folder&quot;,$user,$pass));

}

function pop3_stat($connection)&#xA0; &#xA0; &#xA0; &#xA0; 

{

&#xA0; &#xA0; $check = imap_mailboxmsginfo($connection);

&#xA0; &#xA0; return ((array)$check);

}

function pop3_list($connection,$message=&quot;&quot;)

{

&#xA0; &#xA0; if ($message)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; $range=$message;

&#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; $MC = imap_check($connection);

&#xA0; &#xA0; &#xA0; &#xA0; $range = &quot;1:&quot;.$MC-&gt;Nmsgs;

&#xA0; &#xA0; }

&#xA0; &#xA0; $response = imap_fetch_overview($connection,$range);

&#xA0; &#xA0; foreach ($response as $msg) $result[$msg-&gt;msgno]=(array)$msg;

&#xA0; &#xA0; &#xA0; &#xA0; return $result;

}

function pop3_retr($connection,$message)

{

&#xA0; &#xA0; return(imap_fetchheader($connection,$message,FT_PREFETCHTEXT));

}

function pop3_dele($connection,$message)

{

&#xA0; &#xA0; return(imap_delete($connection,$message));

}

function mail_parse_headers($headers)

{

&#xA0; &#xA0; $headers=preg_replace(&apos;/\r\n\s+/m&apos;, &apos;&apos;,$headers);

&#xA0; &#xA0; preg_match_all(&apos;/([^: ]+): (.+?(?:\r\n\s(?:.+?))*)?\r\n/m&apos;, $headers, $matches);

&#xA0; &#xA0; foreach ($matches[1] as $key =&gt;$value) $result[$value]=$matches[2][$key];

&#xA0; &#xA0; return($result);

}

function mail_mime_to_array($imap,$mid,$parse_headers=false)

{

&#xA0; &#xA0; $mail = imap_fetchstructure($imap,$mid);

&#xA0; &#xA0; $mail = mail_get_parts($imap,$mid,$mail,0);

&#xA0; &#xA0; if ($parse_headers) $mail[0][&quot;parsed&quot;]=mail_parse_headers($mail[0][&quot;data&quot;]);

&#xA0; &#xA0; return($mail);

}

function mail_get_parts($imap,$mid,$part,$prefix)

{&#xA0; &#xA0; 

&#xA0; &#xA0; $attachments=array();

&#xA0; &#xA0; $attachments[$prefix]=mail_decode_part($imap,$mid,$part,$prefix);

&#xA0; &#xA0; if (isset($part-&gt;parts)) // multipart

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; $prefix = ($prefix == &quot;0&quot;)?&quot;&quot;:&quot;$prefix.&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; foreach ($part-&gt;parts as $number=&gt;$subpart) 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $attachments=array_merge($attachments, mail_get_parts($imap,$mid,$subpart,$prefix.($number+1)));

&#xA0; &#xA0; }

&#xA0; &#xA0; return $attachments;

}

function mail_decode_part($connection,$message_number,$part,$prefix)

{

&#xA0; &#xA0; $attachment = array();



&#xA0; &#xA0; if($part-&gt;ifdparameters) {

&#xA0; &#xA0; &#xA0; &#xA0; foreach($part-&gt;dparameters as $object) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $attachment[strtolower($object-&gt;attribute)]=$object-&gt;value;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(strtolower($object-&gt;attribute) == &apos;filename&apos;) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $attachment[&apos;is_attachment&apos;] = true;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $attachment[&apos;filename&apos;] = $object-&gt;value;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }



&#xA0; &#xA0; if($part-&gt;ifparameters) {

&#xA0; &#xA0; &#xA0; &#xA0; foreach($part-&gt;parameters as $object) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $attachment[strtolower($object-&gt;attribute)]=$object-&gt;value;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(strtolower($object-&gt;attribute) == &apos;name&apos;) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $attachment[&apos;is_attachment&apos;] = true;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $attachment[&apos;name&apos;] = $object-&gt;value;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }



&#xA0; &#xA0; $attachment[&apos;data&apos;] = imap_fetchbody($connection, $message_number, $prefix);

&#xA0; &#xA0; if($part-&gt;encoding == 3) { // 3 = BASE64

&#xA0; &#xA0; &#xA0; &#xA0; $attachment[&apos;data&apos;] = base64_decode($attachment[&apos;data&apos;]);

&#xA0; &#xA0; }

&#xA0; &#xA0; elseif($part-&gt;encoding == 4) { // 4 = QUOTED-PRINTABLE

&#xA0; &#xA0; &#xA0; &#xA0; $attachment[&apos;data&apos;] = quoted_printable_decode($attachment[&apos;data&apos;]);

&#xA0; &#xA0; }

&#xA0; &#xA0; return($attachment);

}

?>
```




[EDIT BY danbrown AT php DOT net: Contains a bugfix by &quot;mn26826&quot; on 09-JUN-2010, which fixed the erroneous reference to $imap as the parameter passed to imap_mailboxmsginfo() within the user function pop3_stat().&#xA0; This was intended to be $connection.]



[EDIT BY visualmind AT php DOT net: Contains a bugfix by &quot;elias-jobview&quot; on 17-AUG-2010, which fixed the error in pop3_list function which didn&apos;t have: return $result]



[EDIT BY danbrown AT php DOT net: Contains a bugfix by &quot;chrismeistre&quot; on 09-SEP-2010, which fixed the erroneous reference to $mbox (should be $connection) in the pop3_list() function.]

  

#

[Official documentation page](https://www.php.net/manual/en/book.imap.php)

**[To root](/README.md)**