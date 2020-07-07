# flock





The supplied documentation is vague, ambiguous and lacking, and the user comments contain erroneous information! The flock function follows the semantics of the Unix system call bearing the same name. Flock utilizes ADVISORY locking only; that is, other processes may ignore the lock completely; it only affects those that call the flock call.

LOCK_SH means SHARED LOCK. Any number of processes MAY HAVE A SHARED LOCK simultaneously. It is commonly called a reader lock.

LOCK_EX means EXCLUSIVE LOCK. Only a single process may possess an exclusive lock to a given file at a time.

If the file has been LOCKED with LOCK_SH in another process, flock with LOCK_SH will SUCCEED. flock with LOCK_EX will BLOCK UNTIL ALL READER LOCKS HAVE BEEN RELEASED.

If the file has been locked with LOCK_EX in another process, the CALL WILL BLOCK UNTIL ALL OTHER LOCKS have been released.

If however, you call flock on a file on which you possess the lock, it will try to change it. So: flock(LOCK_EX) followed by flock(LOCK_SH) will get you a SHARED lock, not &quot;read-write&quot; lock.

  

#



Simple Helper for lock files creation



```
<?php

class FileLocker {
&#xA0; &#xA0; protected static $loc_files = array();

&#xA0; &#xA0; public static function lockFile($file_name, $wait = false) {
&#xA0; &#xA0; &#xA0; &#xA0; $loc_file = fopen($file_name, &apos;c&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; if ( !$loc_file ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \Exception(&apos;Can\&apos;t create lock file!&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if ( $wait ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $lock = flock($loc_file, LOCK_EX);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $lock = flock($loc_file, LOCK_EX | LOCK_NB);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if ( $lock ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$loc_files[$file_name] = $loc_file;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fprintf($loc_file, &quot;%s\n&quot;, getmypid());
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $loc_file;
&#xA0; &#xA0; &#xA0; &#xA0; } else if ( $wait ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \Exception(&apos;Can\&apos;t lock file!&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; public static function unlockFile($file_name) {
&#xA0; &#xA0; &#xA0; &#xA0; fclose(self::$loc_files[$file_name]);
&#xA0; &#xA0; &#xA0; &#xA0; @unlink($file_name);
&#xA0; &#xA0; &#xA0; &#xA0; unset(self::$loc_files[$file_name]);
&#xA0; &#xA0; }

} 

if ( !FileLocker::lockFile(&apos;/tmp/1.lock&apos;) ) {
&#xA0; &#xA0; echo &quot;Can&apos;t lock file\n&quot;;
&#xA0; &#xA0; die();
}
sleep(10);
FileLocker::unlockFile(&apos;/tmp/1.lock&apos;);
echo &quot;All Ok\n&quot;;


  

#



Regarding the change in PHP 5.3.2 with locked files:

Without having studied the PHP source code in detail, the situation appears to be as follows when the PHP function fclose() is called:

Before 5.3.2 PHP would check if the file was locked, then release the lock, and then close the file.

From 5.3.2 PHP just closes the file.

But note, that the operating system releases the lock automatically when the file is closed. Therefore a call to fclose() STILL releases the lock (this is tested with PHP 5.3.2, Linux, x64).

  

#



I just spent some time (again) to understand why a reading with file_get_contents() and file was returning me an empty string &quot;&quot; or array() whereas the file was existing and the contents not empty.



In fact, i was locking file when writing it (file_put_contents third arg) but not testing if file was locked when reading it (and the file was accessed a lot).



So, please pay attention that file_get_contents(), file() and maybe others php files functions are going to return empty data like if the contents of the file was an empty string.



To avoid this problem, you have to set a LOCK_SH on your file before reading it (and then waiting if locked).



Something like this :





```
<?php

public static function getContents($path, $waitIfLocked = true) {

&#xA0; &#xA0; if(!file_exists($path)) {

&#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;File &quot;&apos;.$path.&apos;&quot; does not exists&apos;);

&#xA0; &#xA0; }

&#xA0; &#xA0; else {

&#xA0; &#xA0; &#xA0; &#xA0; $fo = fopen($path, &apos;r&apos;);

&#xA0; &#xA0; &#xA0; &#xA0; $locked = flock($fo, LOCK_SH, $waitIfLocked);

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; if(!$locked) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cts = file_get_contents($path);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; flock($fo, LOCK_UN);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($fo);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $cts;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}

?>
```




Code to test by yourself :



abc.txt :

someText



file.php :



```
<?php

$fo = fopen(&apos;abc.txt&apos;, &apos;r+&apos;);



flock($fo, LOCK_EX);

sleep(10);

flock($fo, LOCK_UN);

?>
```




file2.php : 



```
<?php

var_dump(file_get_contents(&apos;abc.txt&apos;));

var_dump(file(&apos;abc.txt&apos;));

?>
```




Then launch file.php and switch to file2.php during the 10 seconds and see the difference before/after

  

#

[Official documentation page](https://www.php.net/manual/en/function.flock.php)

**[To root](/README.md)**