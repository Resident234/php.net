# ob_flush





some problems with ob_flush() and flush() could be resolved by defining content type header :
header( &apos;Content-type: text/html; charset=utf-8&apos; );

so working code looks like this:


```
<?php
header( &apos;Content-type: text/html; charset=utf-8&apos; );
echo &apos;Begin ...&lt;br /&gt;&apos;;
for( $i = 0 ; $i &lt; 10 ; $i++ )
{
&#xA0; &#xA0; echo $i . &apos;&lt;br /&gt;&apos;;
&#xA0; &#xA0; flush();
&#xA0; &#xA0; ob_flush();
&#xA0; &#xA0; sleep(1);
}
echo &apos;End ...&lt;br /&gt;&apos;;
?>
```



  

#



As of August 2012, all browsers seem to show an all-or-nothing approach to buffering. In other words, while php is operating, no content can be shown.

In particular this means that the following workarounds listed further down here are ineffective:

1) ob_flush (),&#xA0; flush () in any combination with other output buffering functions;

2) changes to php.ini involving setting output_buffer and/or zlib.output_compression to 0 or Off;

3) setting Apache variables such as &quot;no-gzip&quot; either through apache_setenv () or through entries in .htaccess.

So, until browsers begin to show buffered content again, the tips listed here are moot.

  

#



Although browsers now have an all or none buffering strategy, the arguments are not moot.

If you are not using ob_flush, you run this risk of exceeding socket timeouts (commonly seen in php-fpm/nginx combos).

Basically, flushing solves the infamous 504 Gateway Time-out error.

  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-flush.php)

**[To root](/README.md)**