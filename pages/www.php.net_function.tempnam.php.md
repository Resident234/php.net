# tempnam





Watch out using a blank $dir as a &quot;trick&quot; to create temporary files in the system temporary directory.





```
<?php

$tmpfname = tempnam(&apos;&apos;, &apos;FOO&apos;); // not good

?>
```




If an open_basedir restriction is in effect, the trick will not work. You will get a warning message like



Warning: tempnam() [function.tempnam]: open_basedir restriction in effect.

File() is not within the allowed path(s): (/var/www/vhosts/example.com/httpdocs:/tmp)



What works is this:





```
<?php

$tmpfname = tempnam(sys_get_temp_dir(), &apos;FOO&apos;); // good

?>
```



  

#



Please note that this function might throw a notice in PHP 7.1.0 and above. This was a bugfix: https://bugs.php.net/bug.php?id=69489

You can place an address operator (@) to sillence the notice:



```
<?php

if ($tmp = @tempnam() !== false) {
&#xA0; // ...
}

?>
```


Or you could try to set the &quot;upload_tmp_dir&quot; setting in your php.ini to the temporary folder path of your system. Not sure, if the last one prevents the notices.

  

#



Note that tempnam returns the full path to the temporary file, not just the filename.

  

#



tempnam() function does not support custom stream wrappers registered by stream_register_wrapper(). 

For example if you&apos;ll try to use tempnam() on Windows platform, PHP will try to generate unique filename in %TMP% folder (usually: C:\WINDOWS\Temp) without any warning or notice.



```
<?php

// &lt;&lt; ...custom stream wrapper goes somewhere here...&gt;&gt;

echo &apos;&lt;pre&gt;&apos;;
error_reporting(E_ALL);
ini_set(&apos;display_errors&apos;, true);
clearstatcache();
stream_register_wrapper(&apos;test&apos;, &apos;MemoryStream&apos;);

mkdir(&apos;test://aaa&apos;);
mkdir(&apos;test://aaa/cc&apos;);
mkdir(&apos;test://aaa/dd&apos;); 
echo &apos;PHP &apos;.PHP_VERSION;
echo &apos;&lt;br /&gt;node exists: &apos;.file_exists(&apos;test://aaa/cc&apos;);
echo &apos;&lt;br /&gt;node is writable: &apos;.is_writable(&apos;test://aaa/cc&apos;);
echo &apos;&lt;br /&gt;node is dir: &apos;.is_dir(&apos;test://aaa/cc&apos;);
echo &apos;&lt;br /&gt;tempnam in dir: &apos;.tempnam(&apos;test://aaa/cc&apos;, &apos;tmp&apos;);
echo &quot;&lt;br /&gt;&lt;/pre&gt;&quot;;

?>
```


ouputs:
--------------------
PHP 5.2.13
node exists: 1
node is writable: 1
node is dir: 1
tempnam in dir: C:\Windows\Temp\tmp1D03.tmp

If you want to create temporary file, you have to create your own function (which will probably use opendir() and fopen($filename, &quot;x&quot;) functions)

  

#



If you go to the linux man page for the C function tempnam(3), you will see at the end &quot;Never use this function. Use mkstemp(3) instead.&quot; But php&apos;s tempnam() function doesn&apos;t actually use tmpnam(3), so there&apos;s no problem (under Linux, it will use mkstemp(3) if it&apos;s available).

  

#

[Official documentation page](https://www.php.net/manual/en/function.tempnam.php)

**[To root](/README.md)**