# rename



Code first, then explanation.<br><br>

```
<?php

 rename ("/folder/file.ext", "newfile.ext");

?>
```


That doesn&apos;t rename the file within the folder, as you might assume, instead, it moves the file to whatever the PHP working directory is... Chances are you&apos;ll not find it in your FTP tree. Instead, use the following:



```
<?php

 rename ("/folder/file.ext", "/folder/newfile.ext");

?>
```
  

#

From the Changelog notes:<br><br>"Warnings may be generated if the destination filesystem doesn&apos;t permit chown() or chmod() system calls to be made on files &#x2014; for example, if the destination filesystem is a FAT filesystem."<br><br>More explicitly, rename() may still return (bool) true, despite the warnings that result from the underlying calls to chown() or chmod(). This behavior can be misleading absent a deeper understanding of the underlying mechanics. To rename across filesystems, PHP "fakes it" by calling copy(), unlink(), chown(), and chmod() (not necessarily in that order). See PHP bug #50676 for more information.<br><br>On UNIX-like operating systems, filesystems may be mounted with an explicit uid and/or gid (for example, with mount options "uid=someuser,gid=somegroup"). Attempting to call rename() with such a destination filesystem will cause an "Operation not permitted" warning, even though the file is indeed renamed and rename() returns (bool) true.<br><br>This is not a bug. Either handle the warning as is appropriate to your use-case, or call copy() and then unlink(), which will avoid the doomed calls to chown() and chmod(), thereby eliminating the warning.  

#

For those who are still confused about the behavior of rename() in Linux and Windows (Windows XP) when target destination exists:<br><br>I have tested rename($oldName, $targetName) in PHP 5.3.0 and PHP 5.2.9 on both OS and find that if a file named $targetName does exist before, it will be overwritten with the content of $oldName  

#

Note, that on Unix, a rename is a beautiful way of getting atomic updates to files.<br><br>Just copy the old contents (if necessary), and write the new contents into a new file, then rename over the original file.<br><br>Any processes reading from the file will continue to do so, any processes trying to open the file while you&apos;re writing to it will get the old file (because you&apos;ll be writing to a temp file), and there is no "intermediate" time between there being a file, and there not being a file (or there being half a file).<br><br>Oh, and this only works if you have the temp file and the destination file on the same filesystem (eg. partition/hard-disk).  

#

rename() is working on Linux/UNIX but not working on Windows on a directory containing a file formerly opened within the same script. The problem persists even after properly closing the file and flushing the buffer.<br><br>

```
<?php
$fp = fopen ("./dir/ex.txt" , "r+");
$text = fread($fp, filesize("../dir/ex.txt"));
ftruncate($fp, 0);
$text2 = "some value";
fwrite ($fp,  $text2 );
fflush($fp);
fclose ($fp);
if (is_resource($fp))
    fclose($fp);
rename ("./dir", ".dir2"); // will give a &#xAB;permission denied&#xBB; error
?>
```


Strangely it seem that the rename command is  executed BEFORE the handle closing on Windows.

Inserting a sleep() command before the renaming cures the problem.



```
<?php
$fp = fopen ("./dir/ex.txt" , "r+");
$text = fread($fp, filesize("../dir/ex.txt"));
ftruncate($fp, 0);
$text2 = "some value";
fwrite ($fp,  $text2 );
fflush($fp);
fclose ($fp);
if (is_resource($fp))
    fclose($fp);
sleep(1);    // this does the trick
rename ("./dir", ".dir2"); //no error 
?>
```
  

#

If by any chance you end up with something equivalent to this:<br><br>

```
<?php
rename(&apos;/foo/bar&apos;,&apos;/foo/bar&apos;);
?>
```
<br><br>It returns true. It&apos;s not documented.  

#

[Official documentation page](https://www.php.net/manual/en/function.rename.php)

**[To root](/README.md)**