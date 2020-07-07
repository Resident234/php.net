# imap_fetchstructure





Here is code to parse and decode all types of messages, including attachments. I&apos;ve been using something like this for awhile now, so it&apos;s pretty robust.

&lt;?
function getmsg($mbox,$mid) {
&#xA0; &#xA0; // input $mbox = IMAP stream, $mid = message id
&#xA0; &#xA0; // output all the following:
&#xA0; &#xA0; global $charset,$htmlmsg,$plainmsg,$attachments;
&#xA0; &#xA0; $htmlmsg = $plainmsg = $charset = &apos;&apos;;
&#xA0; &#xA0; $attachments = array();

&#xA0; &#xA0; // HEADER
&#xA0; &#xA0; $h = imap_header($mbox,$mid);
&#xA0; &#xA0; // add code here to get date, from, to, cc, subject...

&#xA0; &#xA0; // BODY
&#xA0; &#xA0; $s = imap_fetchstructure($mbox,$mid);
&#xA0; &#xA0; if (!$s-&gt;parts)&#xA0; // simple
&#xA0; &#xA0; &#xA0; &#xA0; getpart($mbox,$mid,$s,0);&#xA0; // pass 0 as part-number
&#xA0; &#xA0; else {&#xA0; // multipart: cycle through each part
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($s-&gt;parts as $partno0=&gt;$p)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; getpart($mbox,$mid,$p,$partno0+1);
&#xA0; &#xA0; }
}

function getpart($mbox,$mid,$p,$partno) {
&#xA0; &#xA0; // $partno = &apos;1&apos;, &apos;2&apos;, &apos;2.1&apos;, &apos;2.1.3&apos;, etc for multipart, 0 if simple
&#xA0; &#xA0; global $htmlmsg,$plainmsg,$charset,$attachments;

&#xA0; &#xA0; // DECODE DATA
&#xA0; &#xA0; $data = ($partno)?
&#xA0; &#xA0; &#xA0; &#xA0; imap_fetchbody($mbox,$mid,$partno):&#xA0; // multipart
&#xA0; &#xA0; &#xA0; &#xA0; imap_body($mbox,$mid);&#xA0; // simple
&#xA0; &#xA0; // Any part may be encoded, even plain text messages, so check everything.
&#xA0; &#xA0; if ($p-&gt;encoding==4)
&#xA0; &#xA0; &#xA0; &#xA0; $data = quoted_printable_decode($data);
&#xA0; &#xA0; elseif ($p-&gt;encoding==3)
&#xA0; &#xA0; &#xA0; &#xA0; $data = base64_decode($data);

&#xA0; &#xA0; // PARAMETERS
&#xA0; &#xA0; // get all parameters, like charset, filenames of attachments, etc.
&#xA0; &#xA0; $params = array();
&#xA0; &#xA0; if ($p-&gt;parameters)
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($p-&gt;parameters as $x)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $params[strtolower($x-&gt;attribute)] = $x-&gt;value;
&#xA0; &#xA0; if ($p-&gt;dparameters)
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($p-&gt;dparameters as $x)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $params[strtolower($x-&gt;attribute)] = $x-&gt;value;

&#xA0; &#xA0; // ATTACHMENT
&#xA0; &#xA0; // Any part with a filename is an attachment,
&#xA0; &#xA0; // so an attached text file (type 0) is not mistaken as the message.
&#xA0; &#xA0; if ($params[&apos;filename&apos;] || $params[&apos;name&apos;]) {
&#xA0; &#xA0; &#xA0; &#xA0; // filename may be given as &apos;Filename&apos; or &apos;Name&apos; or both
&#xA0; &#xA0; &#xA0; &#xA0; $filename = ($params[&apos;filename&apos;])? $params[&apos;filename&apos;] : $params[&apos;name&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; // filename may be encoded, so see imap_mime_header_decode()
&#xA0; &#xA0; &#xA0; &#xA0; $attachments[$filename] = $data;&#xA0; // this is a problem if two files have same name
&#xA0; &#xA0; }

&#xA0; &#xA0; // TEXT
&#xA0; &#xA0; if ($p-&gt;type==0 &amp;&amp; $data) {
&#xA0; &#xA0; &#xA0; &#xA0; // Messages may be split in different parts because of inline attachments,
&#xA0; &#xA0; &#xA0; &#xA0; // so append parts together with blank row.
&#xA0; &#xA0; &#xA0; &#xA0; if (strtolower($p-&gt;subtype)==&apos;plain&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $plainmsg. = trim($data) .&quot;\n\n&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $htmlmsg. = $data .&quot;&lt;br&gt;&lt;br&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $charset = $params[&apos;charset&apos;];&#xA0; // assume all parts are same charset
&#xA0; &#xA0; }

&#xA0; &#xA0; // EMBEDDED MESSAGE
&#xA0; &#xA0; // Many bounce notifications embed the original message as type 2,
&#xA0; &#xA0; // but AOL uses type 1 (multipart), which is not handled here.
&#xA0; &#xA0; // There are no PHP functions to parse embedded messages,
&#xA0; &#xA0; // so this just appends the raw source to the main message.
&#xA0; &#xA0; elseif ($p-&gt;type==2 &amp;&amp; $data) {
&#xA0; &#xA0; &#xA0; &#xA0; $plainmsg. = $data.&quot;\n\n&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; // SUBPART RECURSION
&#xA0; &#xA0; if ($p-&gt;parts) {
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($p-&gt;parts as $partno0=&gt;$p2)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; getpart($mbox,$mid,$p2,$partno.&apos;.&apos;.($partno0+1));&#xA0; // 1.2, 1.2.1, etc.
&#xA0; &#xA0; }
}
?>
```


  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-fetchstructure.php)

**[To root](/README.md)**