# Examples





All these examples will not work if the php script has no write access within the folder. 

Although you may say this is obvious, I found that in this case, $zip-&gt;open(&quot;name&quot;, ZIPARCHIVE::CREATE) doesn&apos;t return an error as it might not try to access the file system but rather allocates memory. 

It is only $zip-&gt;close() that returns the error. This might cause you seeking at the wrong end.

  

#





```
<?php

&#xA0; &#xA0; &#xA0; &#xA0; $zip = new ZipArchive;

&#xA0; &#xA0; $zip-&gt;open(&apos;teste.zip&apos;);

&#xA0; &#xA0; $zip-&gt;extractTo(&apos;./&apos;);

&#xA0; &#xA0; $zip-&gt;close();

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Ok!&quot;;

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/zip.examples.php)

**[To root](/README.md)**