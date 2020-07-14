# unlink



This will delete all files in a directory matching a pattern in one line of code.<br><br>

```
<?php array_map(&apos;unlink&apos;, glob("some/dir/*.txt")); ?>
```
  

#

Deleted a large file but seeing no increase in free space or decrease of disk usage? Using UNIX or other POSIX OS?<br><br>The unlink() is not about removing file, it&apos;s about removing a file name. The manpage says: ``unlink - delete a name and possibly the file it refers to&apos;&apos;.<br><br>Most of the time a file has just one name -- removing it will also remove (free, deallocate) the `body&apos; of file (with one caveat, see below). That&apos;s the simple, usual case.<br><br>However, it&apos;s perfectly fine for a file to have several names (see the link() function), in the same or different directories. All the names will refer to the file body and `keep it alive&apos;, so to say. Only when all the names are removed, the body of file actually is freed.<br><br>The caveat:<br>A file&apos;s body may *also* be `kept alive&apos; (still using diskspace) by a process holding the file open. The body will not be deallocated (will not free disk space) as long as the process holds it open. In fact, there&apos;s a fancy way of resurrecting a file removed by a mistake but still held open by a process...  

#

unlink($fileName); failed for me .<br>Then i tried using the realpath($fileName)  function as <br>unlink(realpath($fileName)); it worked <br><br>just posting it , in case if any one finds it useful .  

#

I have been working on some little tryout where a backup file was created before modifying the main textfile. Then when an error is thrown, the main file will be deleted (unlinked) and the backup file is returned instead.<br><br>Though, I have been breaking my head for about an hour on why I couldn&apos;t get my persmissions right to unlink the main file.<br><br>Finally I knew what was wrong: because I was working on the file and hadn&apos;t yet closed the file, it was still in use and ofcourse couldn&apos;t be deleted :)<br><br>So I thought of mentoining this here, to avoid others of making the same mistake:<br><br>

```
<?php
// First close the file
fclose($fp);

// Then unlink :)
unlink($somefile);
?>
```
  

#

To delete all files of a particular extension, or infact, delete all with wildcard, a much simplar way is to use the glob function.  Say I wanted to delete all jpgs .........<br><br>

```
<?php

foreach (glob("*.jpg") as $filename) {
   echo "$filename size " . filesize($filename) . "\n";
   unlink($filename);
}

?>
```
  

#

Here the simplest way to delete files with mask<br><br>

```
<?php
   $mask = "*.jpg"
   array_map( "unlink", glob( $mask ) );
?>
```
  

#

To anyone who&apos;s had a problem with the permissions denied error, it&apos;s sometimes caused when you try to delete a file that&apos;s in a folder higher in the hierarchy to your working directory (i.e. when trying to delete a path that starts with "../").<br><br>So to work around this problem, you can use chdir() to change the working directory to the folder where the file you want to unlink is located.<br><br>

```
<?php
    $old = getcwd(); // Save the current directory
    chdir($path_to_file);
    unlink($filename);
    chdir($old); // Restore the old working directory    
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.unlink.php)

**[To root](/README.md)**