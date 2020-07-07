# zlib://





One-liners to gzip and ungzip a file:

copy(&apos;file.txt&apos;, &apos;compress.zlib://&apos; . &apos;file.txt.gz&apos;);

copy(&apos;compress.zlib://&apos; . &apos;file.txt.gz&apos;, &apos;file.txt&apos;);

  

#



Example on how to read an entry from a ZIP archive (file &quot;bar.txt&quot; inside &quot;./foo.zip&quot;):





```
<?php



$fp = fopen(&apos;zip://./foo.zip#bar.txt&apos;, &apos;r&apos;);

if( $fp ){

&#xA0; &#xA0; while( !feof($fp) ){

&#xA0; &#xA0; &#xA0; &#xA0; echo fread($fp, 8192);

&#xA0; &#xA0; }

&#xA0; &#xA0; fclose($fp);

}



?>
```




Also, apparently, the &quot;zip:&quot; wrapper does not allow writing as of PHP/5.3.6. You can read http://php.net/ziparchive-getstream for further reference since the underlying code is probably the same.

  

#

[Official documentation page](https://www.php.net/manual/en/wrappers.compression.php)

**[To root](/README.md)**