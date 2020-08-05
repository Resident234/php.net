# imap_fetchbody



imap-fetchbody() will decode attached email messages inline with the rest of the email parts, however the way it works when handling attached email messages is inconsistent with the main email message.<br><br>With an email message that only has a text body and does not have any mime attachments, imap-fetchbody() will return the following for each requested part number:<br><br>(empty) - Entire message<br>0 - Message header<br>1 - Body text<br><br>With an email message that is a multi-part message in MIME format, and contains the message text in plain text and HTML, and has a file.ext attachment, imap-fetchbody() will return something like the following for each requested part number:<br><br>(empty) - Entire message<br>0 - Message header<br>1 - MULTIPART/ALTERNATIVE<br>1.1 - TEXT/PLAIN<br>1.2 - TEXT/HTML<br>2 - file.ext<br><br>Now if you attach the above email to an email with the message text in plain text and HTML, imap_fetchbody() will use this type of part number system:<br><br>(empty) - Entire message<br>0 - Message header<br>1 - MULTIPART/ALTERNATIVE<br>1.1 - TEXT/PLAIN<br>1.2 - TEXT/HTML<br>2 - MESSAGE/RFC822 (entire attached message)<br>2.0 - Attached message header<br>2.1 - TEXT/PLAIN<br>2.2 - TEXT/HTML<br>2.3 - file.ext<br><br>Note that the file.ext is on the same level now as the plain text and HTML, and that there is no way to access the MULTIPART/ALTERNATIVE in the attached message.<br><br>Here is a modified version of some of the code from previous posts that will build an easily accessible array that includes accessible attached message parts and the message body if there aren&apos;t multipart mimes.  The $structure variable is the output of the imap_fetchstructure() function.  The returned $part_array has the field &apos;part_number&apos; which contains the part number to be fed directly into the imap_fetchbody() function.<br><br>

```
<?php
function create_part_array($structure, $prefix="") {
    //print_r($structure);
    if (sizeof($structure->parts) > 0) {    // There some sub parts
        foreach ($structure->parts as $count => $part) {
            add_part_to_array($part, $prefix.($count+1), $part_array);
        }
    }else{    // Email does not have a seperate mime attachment for text
        $part_array[] = array('part_number' => $prefix.'1', 'part_object' => $obj);
    }
   return $part_array;
}
// Sub function for create_part_array(). Only called by create_part_array() and itself. 
function add_part_to_array($obj, $partno, &amp; $part_array) {
    $part_array[] = array('part_number' => $partno, 'part_object' => $obj);
    if ($obj->type == 2) { // Check to see if the part is an attached email message, as in the RFC-822 type
        //print_r($obj);
        if (sizeof($obj->parts) > 0) {    // Check to see if the email has parts
            foreach ($obj->parts as $count => $part) {
                // Iterate here again to compensate for the broken way that imap_fetchbody() handles attachments
                if (sizeof($part->parts) > 0) {
                    foreach ($part->parts as $count2 => $part2) {
                        add_part_to_array($part2, $partno.".".($count2+1), $part_array);
                    }
                }else{    // Attached email does not have a seperate mime attachment for text
                    $part_array[] = array('part_number' => $partno.'.'.($count+1), 'part_object' => $obj);
                }
            }
        }else{    // Not sure if this is possible
            $part_array[] = array('part_number' => $prefix.'.1', 'part_object' => $obj);
        }
    }else{    // If there are more sub-parts, expand them out.
        if (sizeof($obj->parts) > 0) {
            foreach ($obj->parts as $count => $p) {
                add_part_to_array($p, $partno.".".($count+1), $part_array);
            }
        }
    }
}
?>
```
  

---

If text has been encoded as quoted-printable (most body text is encoded as this), it must be decoded for it to be displayed correctly (without &apos;=&apos;, &apos;=20&apos; and other strange text chunks all through the string).<br><br>To decode, you can use the following built-in php function...<br><br>quoted_printable_decode($string)<br><br>Hopefully I&apos;ve just saved a few people from having to do a preg_replace on there email bodies.  

---

[Official documentation page](https://www.php.net/manual/en/function.imap-fetchbody.php)

**[To root](/README.md)**