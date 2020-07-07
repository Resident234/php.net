# file





To write all the lines of the file in other words to read the file line by line you can write the code like this:


```
<?php
$names=file(&apos;name.txt&apos;);
// To check the number of lines 
echo count($names).&apos;&lt;br&gt;&apos;;
foreach($names as $name)
{
&#xA0;&#xA0; echo $name.&apos;&lt;br&gt;&apos;;
}
?>
```


this example is so basic to understand how it&apos;s working. I hope it will help many beginners.

Regards,
Bingo

  

#



this may be obvious, but it took me a while to figure out what I was doing wrong. So I wanted to share. I have a file on my &quot;c:\&quot; drive. How do I file() it? 

Don&apos;t forget the backslash is special and you have to &quot;escape&quot; the backslash i.e. &quot;\\&quot;:



```
<?php

$lines = file(&quot;C:\\Documents and Settings\\myfile.txt&quot;);

foreach($lines as $line)
{
&#xA0; &#xA0; echo($line);
}

?>
```
 

hope this helps...

  

#



If the file you are reading is in CSV format do not use file(), use fgetcsv().&#xA0; file() will split the file by each newline that it finds, even newlines that appear within a field (i.e. within quotations).

  

#

[Official documentation page](https://www.php.net/manual/en/function.file.php)

**[To root](/README.md)**