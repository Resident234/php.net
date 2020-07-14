# rmdir



Glob function doesn&apos;t return the hidden files, therefore scandir can be more useful, when trying to delete recursively a tree.<br><br>

```
<?php
public static function delTree($dir) {
   $files = array_diff(scandir($dir), array(&apos;.&apos;,&apos;..&apos;));
    foreach ($files as $file) {
      (is_dir("$dir/$file")) ? delTree("$dir/$file") : unlink("$dir/$file");
    }
    return rmdir($dir);
  }
?>
```
  

#

Never ever use jurchiks101 at gmail dot com code!!! It contains command injection vulnerability!!!<br>If you want to do it that way, use something like this instead:<br><br>

```
<?php
if (PHP_OS === &apos;Windows&apos;)
{
    exec(sprintf("rd /s /q %s", escapeshellarg($path)));
}
else
{
    exec(sprintf("rm -rf %s", escapeshellarg($path)));
}
?>
```
<br><br>Note the escapeshellarg usage to escape any possible unwanted character, this avoids putting commands in $path variable so the possibility of someone "pwning" the server with this code  

#

The function delTree is dangerous when you dont take really care. I for example always deleted a temporary directory with it. Everthing went fine until the moment where the var containing this temporary directory wasnt set. The var didnt contain the path but an empty string. The function delTree  was called and deleted all the files at my host!<br>So dont use this function when you dont have a proper handling coded. Dont think about using this function only for testing without such a handling.<br>Luckily nothing is lost because I had the local copy...  

#

some implementations of recursive folder delete don&apos;t work so well (some give warnings, other don&apos;t delete hidden files etc).<br><br>this one is working fine:<br>

```
<?php

function rrmdir($src) {
    $dir = opendir($src);
    while(false !== ( $file = readdir($dir)) ) {
        if (( $file != &apos;.&apos; ) &amp;&amp; ( $file != &apos;..&apos; )) {
            $full = $src . &apos;/&apos; . $file;
            if ( is_dir($full) ) {
                rrmdir($full);
            }
            else {
                unlink($full);
            }
        }
    }
    closedir($dir);
    rmdir($src);
}

?>
```
  

#

Another simple way to recursively delete a directory that is not empty:<br><br>

```
<?php
 function rrmdir($dir) {
   if (is_dir($dir)) {
     $objects = scandir($dir);
     foreach ($objects as $object) {
       if ($object != "." &amp;&amp; $object != "..") {
         if (filetype($dir."/".$object) == "dir") rrmdir($dir."/".$object); else unlink($dir."/".$object);
       }
     }
     reset($objects);
     rmdir($dir);
   }
 }
?>
```
  

#

I was working on some Dataoperation, and just wanted to share an OOP method with you.<br><br>It just removes any contents of a Directory but not the target Directory itself! Its really nice if you want to clean a BackupDirectory or Log.<br><br>Also you can test on it if something went wrong or if it just done its Work!<br><br>I have it in a FileHandler class for example, enjoy!<br><br>

```
<?php 

  public function deleteContent($path){
      try{
        $iterator = new DirectoryIterator($path);
        foreach ( $iterator as $fileinfo ) {
          if($fileinfo-&gt;isDot())continue;
          if($fileinfo-&gt;isDir()){
            if(deleteContent($fileinfo-&gt;getPathname()))
              @rmdir($fileinfo-&gt;getPathname());
          }
          if($fileinfo-&gt;isFile()){
            @unlink($fileinfo-&gt;getPathname());
          }
        }
      } catch ( Exception $e ){
         // write log
         return false;
      }
      return true;
    }

?>
```
  

#

It is rather dangerous to recurse into symbolically linked directories. The delTree should be modified to check for links.<br><br>

```
<?php 
public static function delTree($dir) { 
   $files = array_diff(scandir($dir), array(&apos;.&apos;,&apos;..&apos;)); 
    foreach ($files as $file) { 
      (is_dir("$dir/$file") &amp;&amp; !is_link($dir)) ? delTree("$dir/$file") : unlink("$dir/$file"); 
    } 
    return rmdir($dir); 
  } 
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.rmdir.php)

**[To root](/README.md)**