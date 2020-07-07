# rmdir





Glob function doesn&apos;t return the hidden files, therefore scandir can be more useful, when trying to delete recursively a tree.





```
<?php

public static function delTree($dir) {

&#xA0;&#xA0; $files = array_diff(scandir($dir), array(&apos;.&apos;,&apos;..&apos;));

&#xA0; &#xA0; foreach ($files as $file) {

&#xA0; &#xA0; &#xA0; (is_dir(&quot;$dir/$file&quot;)) ? delTree(&quot;$dir/$file&quot;) : unlink(&quot;$dir/$file&quot;);

&#xA0; &#xA0; }

&#xA0; &#xA0; return rmdir($dir);

&#xA0; }

?>
```



  

#



Never ever use jurchiks101 at gmail dot com code!!! It contains command injection vulnerability!!!
If you want to do it that way, use something like this instead:



```
<?php
if (PHP_OS === &apos;Windows&apos;)
{
&#xA0; &#xA0; exec(sprintf(&quot;rd /s /q %s&quot;, escapeshellarg($path)));
}
else
{
&#xA0; &#xA0; exec(sprintf(&quot;rm -rf %s&quot;, escapeshellarg($path)));
}
?>
```


Note the escapeshellarg usage to escape any possible unwanted character, this avoids putting commands in $path variable so the possibility of someone &quot;pwning&quot; the server with this code

  

#



The function delTree is dangerous when you dont take really care. I for example always deleted a temporary directory with it. Everthing went fine until the moment where the var containing this temporary directory wasnt set. The var didnt contain the path but an empty string. The function delTree&#xA0; was called and deleted all the files at my host!
So dont use this function when you dont have a proper handling coded. Dont think about using this function only for testing without such a handling.
Luckily nothing is lost because I had the local copy...

  

#



some implementations of recursive folder delete don&apos;t work so well (some give warnings, other don&apos;t delete hidden files etc).

this one is working fine:


```
<?php

function rrmdir($src) {
&#xA0; &#xA0; $dir = opendir($src);
&#xA0; &#xA0; while(false !== ( $file = readdir($dir)) ) {
&#xA0; &#xA0; &#xA0; &#xA0; if (( $file != &apos;.&apos; ) &amp;&amp; ( $file != &apos;..&apos; )) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $full = $src . &apos;/&apos; . $file;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( is_dir($full) ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; rrmdir($full);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unlink($full);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; closedir($dir);
&#xA0; &#xA0; rmdir($src);
}

?>
```



  

#



Another simple way to recursively delete a directory that is not empty:





```
<?php

 function rrmdir($dir) {

&#xA0;&#xA0; if (is_dir($dir)) {

&#xA0; &#xA0;&#xA0; $objects = scandir($dir);

&#xA0; &#xA0;&#xA0; foreach ($objects as $object) {

&#xA0; &#xA0; &#xA0;&#xA0; if ($object != &quot;.&quot; &amp;&amp; $object != &quot;..&quot;) {

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (filetype($dir.&quot;/&quot;.$object) == &quot;dir&quot;) rrmdir($dir.&quot;/&quot;.$object); else unlink($dir.&quot;/&quot;.$object);

&#xA0; &#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0;&#xA0; reset($objects);

&#xA0; &#xA0;&#xA0; rmdir($dir);

&#xA0;&#xA0; }

 }

?>
```



  

#



I was working on some Dataoperation, and just wanted to share an OOP method with you.

It just removes any contents of a Directory but not the target Directory itself! Its really nice if you want to clean a BackupDirectory or Log.

Also you can test on it if something went wrong or if it just done its Work!

I have it in a FileHandler class for example, enjoy!



```
<?php 

&#xA0; public function deleteContent($path){
&#xA0; &#xA0; &#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0; $iterator = new DirectoryIterator($path);
&#xA0; &#xA0; &#xA0; &#xA0; foreach ( $iterator as $fileinfo ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($fileinfo-&gt;isDot())continue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($fileinfo-&gt;isDir()){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(deleteContent($fileinfo-&gt;getPathname()))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @rmdir($fileinfo-&gt;getPathname());
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($fileinfo-&gt;isFile()){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @unlink($fileinfo-&gt;getPathname());
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; } catch ( Exception $e ){
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // write log
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return false;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; }

?>
```



  

#



It is rather dangerous to recurse into symbolically linked directories. The delTree should be modified to check for links.



```
<?php 
public static function delTree($dir) { 
&#xA0;&#xA0; $files = array_diff(scandir($dir), array(&apos;.&apos;,&apos;..&apos;)); 
&#xA0; &#xA0; foreach ($files as $file) { 
&#xA0; &#xA0; &#xA0; (is_dir(&quot;$dir/$file&quot;) &amp;&amp; !is_link($dir)) ? delTree(&quot;$dir/$file&quot;) : unlink(&quot;$dir/$file&quot;); 
&#xA0; &#xA0; } 
&#xA0; &#xA0; return rmdir($dir); 
&#xA0; } 
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.rmdir.php)

**[To root](/README.md)**