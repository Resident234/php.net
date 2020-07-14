# imap_savebody



By using imap_fetchbody() you may run in trouble by using too much memory. Using imap_savebody() may prevent this. <br><br>But the content will be encoded, in other words it is useless. Adding a filter can help here.<br><br>

```
<?php
$whandle = fopen(&apos;./incomming/tmp.tif&apos;,&apos;w&apos;);

stream_filter_append($whandle, 
   &apos;convert.base64-decode&apos;,STREAM_FILTER_WRITE);

imap_savebody ($mbox, $whandle, $i, $partcounter++);

fclose($whandle);
?>
```
<br><br>NOTE: To find the proper filter you need to check the encoding given by the structure of the body.  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-savebody.php)

**[To root](/README.md)**