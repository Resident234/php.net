# chmod



BEWARE, a couple of the examples in the comments suggest doing something like this:<br><br>chmod(file_or_dir_name, intval($mode, 8));<br><br>However, if $mode is an integer then intval( ) won&apos;t modify it.  So, this code...<br><br>$mode = 644;<br>chmod(&apos;/tmp/test&apos;, intval($mode, 8));<br><br>...produces permissions that look like this:<br><br>1--w----r-T<br><br>Instead, use octdec( ), like this:<br><br>chmod(file_or_dir_name, octdec($mode));<br><br>See also: http://www.php.net/manual/en/function.octdec.php  

#

BEWARE using quotes around the second parameter...<br><br>If you use quotes eg<br><br>chmod (file, "0644");<br><br>php will not complain but will do an implicit conversion to an int before running chmod. Unfortunately the implicit conversion doesn&apos;t take into account the octal string so you end up with an integer version 644, which is 1204 octal  

#

Usefull reference:<br><br>Value    Permission Level<br>400    Owner Read<br>200    Owner Write<br>100    Owner Execute<br>40    Group Read<br>20    Group Write<br>10    Group Execute<br>4    Global Read<br>2    Global Write<br>1    Global Execute<br><br>(taken from http://www.onlamp.com/pub/a/php/2003/02/06/php_foundations.html)  

#

In the previous post, stickybit avenger writes:<br>    Just a little hint. I was once adwised to set the &apos;sticky bit&apos;, i.e. use 1777 as chmod-value...<br><br>Note that in order to set the sticky bit on a file one must use &apos;01777&apos; (oct) and not &apos;1777&apos; (dec) as the parameter to chmod:<br><br>

```
<?php
    chmod("file",01777);   // correct
     chmod("file",1777);    // incorrect, same as chmod("file",01023), causing no owner permissions!
?>
```
<br><br>Rule of thumb: always prefix octal mode values with a zero.  

#

Changes file mode recursive in $pathname to $filemode<br><br>

```
<?php

$iterator = new RecursiveIteratorIterator(new RecursiveDirectoryIterator($pathname));

foreach($iterator as $item) {
    chmod($item, $filemode);
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.chmod.php)

**[To root](/README.md)**