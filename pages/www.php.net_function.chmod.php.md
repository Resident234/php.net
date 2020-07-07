# chmod





BEWARE, a couple of the examples in the comments suggest doing something like this:

chmod(file_or_dir_name, intval($mode, 8));

However, if $mode is an integer then intval( ) won&apos;t modify it.&#xA0; So, this code...

$mode = 644;
chmod(&apos;/tmp/test&apos;, intval($mode, 8));

...produces permissions that look like this:

1--w----r-T

Instead, use octdec( ), like this:

chmod(file_or_dir_name, octdec($mode));

See also: http://www.php.net/manual/en/function.octdec.php

  

#



BEWARE using quotes around the second parameter...

If you use quotes eg

chmod (file, &quot;0644&quot;);

php will not complain but will do an implicit conversion to an int before running chmod. Unfortunately the implicit conversion doesn&apos;t take into account the octal string so you end up with an integer version 644, which is 1204 octal

  

#



Usefull reference:

Value&#xA0; &#xA0; Permission Level
400&#xA0; &#xA0; Owner Read
200&#xA0; &#xA0; Owner Write
100&#xA0; &#xA0; Owner Execute
40&#xA0; &#xA0; Group Read
20&#xA0; &#xA0; Group Write
10&#xA0; &#xA0; Group Execute
4&#xA0; &#xA0; Global Read
2&#xA0; &#xA0; Global Write
1&#xA0; &#xA0; Global Execute

(taken from http://www.onlamp.com/pub/a/php/2003/02/06/php_foundations.html)

  

#



In the previous post, stickybit avenger writes:

&#xA0; &#xA0; Just a little hint. I was once adwised to set the &apos;sticky bit&apos;, i.e. use 1777 as chmod-value...



Note that in order to set the sticky bit on a file one must use &apos;01777&apos; (oct) and not &apos;1777&apos; (dec) as the parameter to chmod:





```
<?php

&#xA0; &#xA0; chmod(&quot;file&quot;,01777);&#xA0;&#xA0; // correct

&#xA0; &#xA0;&#xA0; chmod(&quot;file&quot;,1777);&#xA0; &#xA0; // incorrect, same as chmod(&quot;file&quot;,01023), causing no owner permissions!

?>
```




Rule of thumb: always prefix octal mode values with a zero.

  

#



Changes file mode recursive in $pathname to $filemode



```
<?php

$iterator = new RecursiveIteratorIterator(new RecursiveDirectoryIterator($pathname));

foreach($iterator as $item) {
&#xA0; &#xA0; chmod($item, $filemode);
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.chmod.php)

**[To root](/README.md)**