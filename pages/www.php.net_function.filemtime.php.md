# filemtime



This is a very handy function for dealing with browser caching. For example, say you have a stylesheet and you want to make sure everyone has the most recent version. You could rename it every time you edit it, but that would be a pain in the ass. Instead, you can do this:<br><br>

```
<?php
echo &apos;&lt;link rel="stylesheet" type="text/css" href="style.css?&apos; . filemtime(&apos;style.css&apos;) . &apos;" /&gt;&apos;;
?>
```
<br><br>Sample output:<br><br>&lt;link rel="stylesheet" type="text/css" href="style.css?1203291283" /&gt;<br><br>By appending a GET value (the UNIX timestamp) to the stylesheet URL, you make the browser think the stylesheet is dynamic, so it&apos;ll reload the stylesheet every time the modification date changes.  

#

To get the last modification time of a directory, you can use this:<br><br>&lt;pre&gt;<br>$getLastModDir = filemtime("/path/to/directory/.");<br>&lt;/pre&gt;<br><br>Take note on the last dot which is needed to see the directory as a file and to actually get a last modification date of it.<br><br>This comes in handy when you want just one &apos;last updated&apos; message on the frontpage of your website and still taking all files of your website into account.<br><br>Regards,<br>Frank Keijzers  

#

Cheaper and dirtier way to code a cache:<br><br>

```
<?php
$cache_file = &apos;URI to cache file&apos;;
$cache_life = &apos;120&apos;; //caching time, in seconds

$filemtime = @filemtime($cache_file);  // returns FALSE if file does not exist
if (!$filemtime or (time() - $filemtime &gt;= $cache_life)){
    ob_start();
    resource_consuming_function();
    file_put_contents($cache_file,ob_get_flush());
}else{
    readfile($cache_file);
}
?>
```
  

#

There&apos;s a deeply-seated problem with filemtime() under Windows due to the fact that it calls Windows&apos; stat() function, which implements DST (according to this bug: http://bugs.php.net/bug.php?id=40568). The detection of DST on the time of the file is confused by whether the CURRENT time of the current system is currently under DST.<br><br>This is a fix for the mother of all annoying bugs:<br><br>

```
<?php
function GetCorrectMTime($filePath)
{

    $time = filemtime($filePath);

    $isDST = (date(&apos;I&apos;, $time) == 1);
    $systemDST = (date(&apos;I&apos;) == 1);

    $adjustment = 0;

    if($isDST == false &amp;&amp; $systemDST == true)
        $adjustment = 3600;
    
    else if($isDST == true &amp;&amp; $systemDST == false)
        $adjustment = -3600;

    else
        $adjustment = 0;

    return ($time + $adjustment);
}
?>
```
<br><br>Dustin Oprea  

#

[Official documentation page](https://www.php.net/manual/en/function.filemtime.php)

**[To root](/README.md)**