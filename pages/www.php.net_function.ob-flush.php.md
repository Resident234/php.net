# ob_flush



some problems with ob_flush() and flush() could be resolved by defining content type header :<br>header( &apos;Content-type: text/html; charset=utf-8&apos; );<br><br>so working code looks like this:<br>

```
<?php
header( 'Content-type: text/html; charset=utf-8' );
echo 'Begin ...<br />';
for( $i = 0 ; $i < 10 ; $i++ )
{
    echo $i . '<br />';
    flush();
    ob_flush();
    sleep(1);
}
echo 'End ...<br />';
?>
```
  

#

As of August 2012, all browsers seem to show an all-or-nothing approach to buffering. In other words, while php is operating, no content can be shown.<br><br>In particular this means that the following workarounds listed further down here are ineffective:<br><br>1) ob_flush (),  flush () in any combination with other output buffering functions;<br><br>2) changes to php.ini involving setting output_buffer and/or zlib.output_compression to 0 or Off;<br><br>3) setting Apache variables such as "no-gzip" either through apache_setenv () or through entries in .htaccess.<br><br>So, until browsers begin to show buffered content again, the tips listed here are moot.  

#

Although browsers now have an all or none buffering strategy, the arguments are not moot.<br><br>If you are not using ob_flush, you run this risk of exceeding socket timeouts (commonly seen in ?>
```
fpm/nginx combos).<br><br>Basically, flushing solves the infamous 504 Gateway Time-out error.  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-flush.php)

**[To root](/README.md)**