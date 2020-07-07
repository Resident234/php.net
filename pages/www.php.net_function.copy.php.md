# copy





Having spent hours tacking down a copy() error: Permission denied , (and duly worrying about chmod on winXP) , its worth pointing out that the &apos;destination&apos; needs to contain the actual file name ! --- NOT just the path to the folder you wish to copy into.......
DOH !
hope this saves somebody hours of fruitless debugging

  

#



It take me a long time to find out what the problem is when i&apos;ve got an error on copy(). It DOESN&apos;T create any directories. It only copies to existing path. So create directories before. Hope i&apos;ll help,

  

#



Don&apos;t forget; you can use copy on remote files, rather than doing messy fopen stuff.&#xA0; e.g.



```
<?php
if(!@copy(&apos;http://someserver.com/somefile.zip&apos;,&apos;./somefile.zip&apos;))
{
&#xA0; &#xA0; $errors= error_get_last();
&#xA0; &#xA0; echo &quot;COPY ERROR: &quot;.$errors[&apos;type&apos;];
&#xA0; &#xA0; echo &quot;&lt;br /&gt;\n&quot;.$errors[&apos;message&apos;];
} else {
&#xA0; &#xA0; echo &quot;File copied from remote!&quot;;
}
?>
```



  

#



A nice simple trick if you need to make sure the folder exists first:



```
<?php

$srcfile=&apos;C:\File\Whatever\Path\Joe.txt&apos;;
$dstfile=&apos;G:\Shared\Reports\Joe.txt&apos;;
mkdir(dirname($dstfile), 0777, true);
copy($srcfile, $dstfile);

?>
```


That simple.

  

#



Here is a simple script that I use for removing and copying non-empty directories. Very useful when you are not sure what is the type of a file.

I am using these for managing folders and zip archives for my website plugins.



```
<?php

// removes files and non-empty directories
function rrmdir($dir) {
&#xA0; if (is_dir($dir)) {
&#xA0; &#xA0; $files = scandir($dir);
&#xA0; &#xA0; foreach ($files as $file)
&#xA0; &#xA0; if ($file != &quot;.&quot; &amp;&amp; $file != &quot;..&quot;) rrmdir(&quot;$dir/$file&quot;);
&#xA0; &#xA0; rmdir($dir);
&#xA0; }
&#xA0; else if (file_exists($dir)) unlink($dir);
} 

// copies files and non-empty directories
function rcopy($src, $dst) {
&#xA0; if (file_exists($dst)) rrmdir($dst);
&#xA0; if (is_dir($src)) {
&#xA0; &#xA0; mkdir($dst);
&#xA0; &#xA0; $files = scandir($src);
&#xA0; &#xA0; foreach ($files as $file)
&#xA0; &#xA0; if ($file != &quot;.&quot; &amp;&amp; $file != &quot;..&quot;) rcopy(&quot;$src/$file&quot;, &quot;$dst/$file&quot;); 
&#xA0; }
&#xA0; else if (file_exists($src)) copy($src, $dst);
}
?>
```


Cheers!

  

#



Here&apos;s a simple recursive function to copy entire directories



Note to do your own check to make sure the directory exists that you first call it on.





```
<?php

function recurse_copy($src,$dst) {

&#xA0; &#xA0; $dir = opendir($src);

&#xA0; &#xA0; @mkdir($dst);

&#xA0; &#xA0; while(false !== ( $file = readdir($dir)) ) {

&#xA0; &#xA0; &#xA0; &#xA0; if (( $file != &apos;.&apos; ) &amp;&amp; ( $file != &apos;..&apos; )) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( is_dir($src . &apos;/&apos; . $file) ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; recurse_copy($src . &apos;/&apos; . $file,$dst . &apos;/&apos; . $file);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else { 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; copy($src . &apos;/&apos; . $file,$dst . &apos;/&apos; . $file);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; closedir($dir);

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.copy.php)

**[To root](/README.md)**