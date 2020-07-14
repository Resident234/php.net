# IMAP, POP3 and NNTP



For all the people coming here praying for:<br><br>1) a dead-easy way to read MIME attachments, or<br>2) a dead-easy way to access POP3 folders<br><br>Look no further.<br><br>

```
<?php 
function pop3_login($host,$port,$user,$pass,$folder="INBOX",$ssl=false)
{
    $ssl=($ssl==false)?"/novalidate-cert":"";
    return (imap_open("{"."$host:$port/pop3$ssl"."}$folder",$user,$pass));
}
function pop3_stat($connection)        
{
    $check = imap_mailboxmsginfo($connection);
    return ((array)$check);
}
function pop3_list($connection,$message="")
{
    if ($message)
    {
        $range=$message;
    } else {
        $MC = imap_check($connection);
        $range = "1:".$MC-&gt;Nmsgs;
    }
    $response = imap_fetch_overview($connection,$range);
    foreach ($response as $msg) $result[$msg-&gt;msgno]=(array)$msg;
        return $result;
}
function pop3_retr($connection,$message)
{
    return(imap_fetchheader($connection,$message,FT_PREFETCHTEXT));
}
function pop3_dele($connection,$message)
{
    return(imap_delete($connection,$message));
}
function mail_parse_headers($headers)
{
    $headers=preg_replace(&apos;/\r\n\s+/m&apos;, &apos;&apos;,$headers);
    preg_match_all(&apos;/([^: ]+): (.+?(?:\r\n\s(?:.+?))*)?\r\n/m&apos;, $headers, $matches);
    foreach ($matches[1] as $key =&gt;$value) $result[$value]=$matches[2][$key];
    return($result);
}
function mail_mime_to_array($imap,$mid,$parse_headers=false)
{
    $mail = imap_fetchstructure($imap,$mid);
    $mail = mail_get_parts($imap,$mid,$mail,0);
    if ($parse_headers) $mail[0]["parsed"]=mail_parse_headers($mail[0]["data"]);
    return($mail);
}
function mail_get_parts($imap,$mid,$part,$prefix)
{    
    $attachments=array();
    $attachments[$prefix]=mail_decode_part($imap,$mid,$part,$prefix);
    if (isset($part-&gt;parts)) // multipart
    {
        $prefix = ($prefix == "0")?"":"$prefix.";
        foreach ($part-&gt;parts as $number=&gt;$subpart) 
            $attachments=array_merge($attachments, mail_get_parts($imap,$mid,$subpart,$prefix.($number+1)));
    }
    return $attachments;
}
function mail_decode_part($connection,$message_number,$part,$prefix)
{
    $attachment = array();

    if($part-&gt;ifdparameters) {
        foreach($part-&gt;dparameters as $object) {
            $attachment[strtolower($object-&gt;attribute)]=$object-&gt;value;
            if(strtolower($object-&gt;attribute) == &apos;filename&apos;) {
                $attachment[&apos;is_attachment&apos;] = true;
                $attachment[&apos;filename&apos;] = $object-&gt;value;
            }
        }
    }

    if($part-&gt;ifparameters) {
        foreach($part-&gt;parameters as $object) {
            $attachment[strtolower($object-&gt;attribute)]=$object-&gt;value;
            if(strtolower($object-&gt;attribute) == &apos;name&apos;) {
                $attachment[&apos;is_attachment&apos;] = true;
                $attachment[&apos;name&apos;] = $object-&gt;value;
            }
        }
    }

    $attachment[&apos;data&apos;] = imap_fetchbody($connection, $message_number, $prefix);
    if($part-&gt;encoding == 3) { // 3 = BASE64
        $attachment[&apos;data&apos;] = base64_decode($attachment[&apos;data&apos;]);
    }
    elseif($part-&gt;encoding == 4) { // 4 = QUOTED-PRINTABLE
        $attachment[&apos;data&apos;] = quoted_printable_decode($attachment[&apos;data&apos;]);
    }
    return($attachment);
}
?>
```
<br><br>[EDIT BY danbrown AT php DOT net: Contains a bugfix by "mn26826" on 09-JUN-2010, which fixed the erroneous reference to $imap as the parameter passed to imap_mailboxmsginfo() within the user function pop3_stat().  This was intended to be $connection.]<br><br>[EDIT BY visualmind AT php DOT net: Contains a bugfix by "elias-jobview" on 17-AUG-2010, which fixed the error in pop3_list function which didn&apos;t have: return $result]<br><br>[EDIT BY danbrown AT php DOT net: Contains a bugfix by "chrismeistre" on 09-SEP-2010, which fixed the erroneous reference to $mbox (should be $connection) in the pop3_list() function.]  

#

[Official documentation page](https://www.php.net/manual/en/book.imap.php)

**[To root](/README.md)**