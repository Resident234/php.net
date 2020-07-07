# imap_fetchbody





imap-fetchbody() will decode attached email messages inline with the rest of the email parts, however the way it works when handling attached email messages is inconsistent with the main email message.

With an email message that only has a text body and does not have any mime attachments, imap-fetchbody() will return the following for each requested part number:

(empty) - Entire message
0 - Message header
1 - Body text

With an email message that is a multi-part message in MIME format, and contains the message text in plain text and HTML, and has a file.ext attachment, imap-fetchbody() will return something like the following for each requested part number:

(empty) - Entire message
0 - Message header
1 - MULTIPART/ALTERNATIVE
1.1 - TEXT/PLAIN
1.2 - TEXT/HTML
2 - file.ext

Now if you attach the above email to an email with the message text in plain text and HTML, imap_fetchbody() will use this type of part number system:

(empty) - Entire message
0 - Message header
1 - MULTIPART/ALTERNATIVE
1.1 - TEXT/PLAIN
1.2 - TEXT/HTML
2 - MESSAGE/RFC822 (entire attached message)
2.0 - Attached message header
2.1 - TEXT/PLAIN
2.2 - TEXT/HTML
2.3 - file.ext

Note that the file.ext is on the same level now as the plain text and HTML, and that there is no way to access the MULTIPART/ALTERNATIVE in the attached message.

Here is a modified version of some of the code from previous posts that will build an easily accessible array that includes accessible attached message parts and the message body if there aren&apos;t multipart mimes.&#xA0; The $structure variable is the output of the imap_fetchstructure() function.&#xA0; The returned $part_array has the field &apos;part_number&apos; which contains the part number to be fed directly into the imap_fetchbody() function.



```
<?php
function create_part_array($structure, $prefix=&quot;&quot;) {
&#xA0; &#xA0; //print_r($structure);
&#xA0; &#xA0; if (sizeof($structure-&gt;parts) &gt; 0) {&#xA0; &#xA0; // There some sub parts
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($structure-&gt;parts as $count =&gt; $part) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; add_part_to_array($part, $prefix.($count+1), $part_array);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }else{&#xA0; &#xA0; // Email does not have a seperate mime attachment for text
&#xA0; &#xA0; &#xA0; &#xA0; $part_array[] = array(&apos;part_number&apos; =&gt; $prefix.&apos;1&apos;, &apos;part_object&apos; =&gt; $obj);
&#xA0; &#xA0; }
&#xA0;&#xA0; return $part_array;
}
// Sub function for create_part_array(). Only called by create_part_array() and itself. 
function add_part_to_array($obj, $partno, &amp; $part_array) {
&#xA0; &#xA0; $part_array[] = array(&apos;part_number&apos; =&gt; $partno, &apos;part_object&apos; =&gt; $obj);
&#xA0; &#xA0; if ($obj-&gt;type == 2) { // Check to see if the part is an attached email message, as in the RFC-822 type
&#xA0; &#xA0; &#xA0; &#xA0; //print_r($obj);
&#xA0; &#xA0; &#xA0; &#xA0; if (sizeof($obj-&gt;parts) &gt; 0) {&#xA0; &#xA0; // Check to see if the email has parts
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($obj-&gt;parts as $count =&gt; $part) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Iterate here again to compensate for the broken way that imap_fetchbody() handles attachments
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (sizeof($part-&gt;parts) &gt; 0) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($part-&gt;parts as $count2 =&gt; $part2) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; add_part_to_array($part2, $partno.&quot;.&quot;.($count2+1), $part_array);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }else{&#xA0; &#xA0; // Attached email does not have a seperate mime attachment for text
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $part_array[] = array(&apos;part_number&apos; =&gt; $partno.&apos;.&apos;.($count+1), &apos;part_object&apos; =&gt; $obj);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }else{&#xA0; &#xA0; // Not sure if this is possible
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $part_array[] = array(&apos;part_number&apos; =&gt; $prefix.&apos;.1&apos;, &apos;part_object&apos; =&gt; $obj);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }else{&#xA0; &#xA0; // If there are more sub-parts, expand them out.
&#xA0; &#xA0; &#xA0; &#xA0; if (sizeof($obj-&gt;parts) &gt; 0) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($obj-&gt;parts as $count =&gt; $p) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; add_part_to_array($p, $partno.&quot;.&quot;.($count+1), $part_array);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}
?>
```



  

#



If text has been encoded as quoted-printable (most body text is encoded as this), it must be decoded for it to be displayed correctly (without &apos;=&apos;, &apos;=20&apos; and other strange text chunks all through the string).

To decode, you can use the following built-in php function...

quoted_printable_decode($string)

Hopefully I&apos;ve just saved a few people from having to do a preg_replace on there email bodies.

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-fetchbody.php)

**[To root](/README.md)**