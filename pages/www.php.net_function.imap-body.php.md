# imap_body



Simple example on how to read body message of the recent mail.<br><br>

```
<?php
$imap = imap_open("{pop.example.com:995/pop3/ssl/novalidate-cert}", "username", "password");

if( $imap ) {
    
     //Check no.of.msgs
     $num = imap_num_msg($imap);

     //if there is a message in your inbox
     if( $num >0 ) {
          //read that mail recently arrived
          echo imap_qprint(imap_body($imap, $num));
     }

     //close the stream
     imap_close($imap);
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.imap-body.php)

**[To root](/README.md)**