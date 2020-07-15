# file



To write all the lines of the file in other words to read the file line by line you can write the code like this:<br>

```
<?php
$names=file('name.txt');
// To check the number of lines 
echo count($names).'&lt;br&gt;';
foreach($names as $name)
{
   echo $name.'&lt;br&gt;';
}
?>
```
<br><br>this example is so basic to understand how it&apos;s working. I hope it will help many beginners.<br><br>Regards,<br>Bingo  

#

this may be obvious, but it took me a while to figure out what I was doing wrong. So I wanted to share. I have a file on my "c:\" drive. How do I file() it? <br><br>Don&apos;t forget the backslash is special and you have to "escape" the backslash i.e. "\\":<br><br>

```
<?php

$lines = file("C:\\Documents and Settings\\myfile.txt");

foreach($lines as $line)
{
    echo($line);
}

?>
```
 <br><br>hope this helps...  

#

If the file you are reading is in CSV format do not use file(), use fgetcsv().  file() will split the file by each newline that it finds, even newlines that appear within a field (i.e. within quotations).  

#

[Official documentation page](https://www.php.net/manual/en/function.file.php)

**[To root](/README.md)**