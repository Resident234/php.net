# imap_body





Simple example on how to read body message of the recent mail.





```
<?php

$imap = imap_open(&quot;{pop.example.com:995/pop3/ssl/novalidate-cert}&quot;, &quot;username&quot;, &quot;password&quot;);



if( $imap ) {

&#xA0; &#xA0; 

&#xA0; &#xA0;&#xA0; //Check no.of.msgs

&#xA0; &#xA0;&#xA0; $num = imap_num_msg($imap);



&#xA0; &#xA0;&#xA0; //if there is a message in your inbox

&#xA0; &#xA0;&#xA0; if( $num &gt;0 ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //read that mail recently arrived

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo imap_qprint(imap_body($imap, $num));

&#xA0; &#xA0;&#xA0; }



&#xA0; &#xA0;&#xA0; //close the stream

&#xA0; &#xA0;&#xA0; imap_close($imap);

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-body.php)

**[To root](/README.md)**