# basename





Support of the $suffix parameter has changed between PHP4 and PHP5:
in PHP4, $suffix is removed first, and then the core basename is applied.
conversely, in PHP5, $suffix is removed AFTER applying core basename.

Example:


```
<?php
&#xA0; $file = &quot;path/to/file.xml#xpointer(/Texture)&quot;;
&#xA0; echo basename($file, &quot;.xml#xpointer(/Texture)&quot;);
?>
```


Result in PHP4: file
Result in PHP5: Texture)

  

#



It&apos;s a shame, that for a 20 years of development we don&apos;t have mb_basename() yet!

// works both in windows and unix
function mb_basename($path) {
&#xA0; &#xA0; if (preg_match(&apos;@^.*[\\\\/]([^\\\\/]+)$@s&apos;, $path, $matches)) {
&#xA0; &#xA0; &#xA0; &#xA0; return $matches[1];
&#xA0; &#xA0; } else if (preg_match(&apos;@^([^\\\\/]+)$@s&apos;, $path, $matches)) {
&#xA0; &#xA0; &#xA0; &#xA0; return $matches[1];
&#xA0; &#xA0; }
&#xA0; &#xA0; return &apos;&apos;;
}

  

#



There is only one variant that works in my case for my Russian UTF-8 letters:





```
<?php

function mb_basename($file)

{

&#xA0; &#xA0; return end(explode(&apos;/&apos;,$file));

}

&gt;&lt;



It is intented for UNIX servers


  

#

[Official documentation page](https://www.php.net/manual/en/function.basename.php)

**[To root](/README.md)**