# ZipArchive::setPassword





It seems that this function supports only decryption of password protected archives (see changelog: http://pecl.php.net/package-changelog.php?package=zip). Creation of password protected archives is not supported (they will be created simply as non-protected archives).

Example code for extraction of files from password protected ZIP archives:



```
<?php
&#xA0; &#xA0; $zip = new ZipArchive();
&#xA0; &#xA0; $zip_status = $zip-&gt;open(&quot;test.zip&quot;);

&#xA0; &#xA0; if ($zip_status === true)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if ($zip-&gt;setPassword(&quot;MySecretPassword&quot;))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$zip-&gt;extractTo(__DIR__))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Extraction failed (wrong password?)&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; $zip-&gt;close();
&#xA0; &#xA0; }
&#xA0; &#xA0; else
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; die(&quot;Failed opening archive: &quot;. @$zip-&gt;getStatusString() . &quot; (code: &quot;. $zip_status .&quot;)&quot;);
&#xA0; &#xA0; }
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.setpassword.php)

**[To root](/README.md)**