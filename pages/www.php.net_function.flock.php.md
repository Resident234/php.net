# flock



The supplied documentation is vague, ambiguous and lacking, and the user comments contain erroneous information! The flock function follows the semantics of the Unix system call bearing the same name. Flock utilizes ADVISORY locking only; that is, other processes may ignore the lock completely; it only affects those that call the flock call.<br><br>LOCK_SH means SHARED LOCK. Any number of processes MAY HAVE A SHARED LOCK simultaneously. It is commonly called a reader lock.<br><br>LOCK_EX means EXCLUSIVE LOCK. Only a single process may possess an exclusive lock to a given file at a time.<br><br>If the file has been LOCKED with LOCK_SH in another process, flock with LOCK_SH will SUCCEED. flock with LOCK_EX will BLOCK UNTIL ALL READER LOCKS HAVE BEEN RELEASED.<br><br>If the file has been locked with LOCK_EX in another process, the CALL WILL BLOCK UNTIL ALL OTHER LOCKS have been released.<br><br>If however, you call flock on a file on which you possess the lock, it will try to change it. So: flock(LOCK_EX) followed by flock(LOCK_SH) will get you a SHARED lock, not "read-write" lock.  

#

Simple Helper for lock files creation<br><br>

```
<?php<br><br>class FileLocker {<br>    protected static $loc_files = array();<br><br>    public static function lockFile($file_name, $wait = false) {<br>        $loc_file = fopen($file_name, &apos;c&apos;);<br>        if ( !$loc_file ) {<br>            throw new \Exception(&apos;Can\&apos;t create lock file!&apos;);<br>        }<br>        if ( $wait ) {<br>            $lock = flock($loc_file, LOCK_EX);<br>        } else {<br>            $lock = flock($loc_file, LOCK_EX | LOCK_NB);<br>        }<br>        if ( $lock ) {<br>            self::$loc_files[$file_name] = $loc_file;<br>            fprintf($loc_file, "%s\n", getmypid());<br>            return $loc_file;<br>        } else if ( $wait ) {<br>            throw new \Exception(&apos;Can\&apos;t lock file!&apos;);<br>        } else {<br>            return false;<br>        }<br>    }<br><br>    public static function unlockFile($file_name) {<br>        fclose(self::$loc_files[$file_name]);<br>        @unlink($file_name);<br>        unset(self::$loc_files[$file_name]);<br>    }<br><br>} <br><br>if ( !FileLocker::lockFile(&apos;/tmp/1.lock&apos;) ) {<br>    echo "Can&apos;t lock file\n";<br>    die();<br>}<br>sleep(10);<br>FileLocker::unlockFile(&apos;/tmp/1.lock&apos;);<br>echo "All Ok\n";  

#

Regarding the change in PHP 5.3.2 with locked files:<br><br>Without having studied the PHP source code in detail, the situation appears to be as follows when the PHP function fclose() is called:<br><br>Before 5.3.2 PHP would check if the file was locked, then release the lock, and then close the file.<br><br>From 5.3.2 PHP just closes the file.<br><br>But note, that the operating system releases the lock automatically when the file is closed. Therefore a call to fclose() STILL releases the lock (this is tested with PHP 5.3.2, Linux, x64).  

#

I just spent some time (again) to understand why a reading with file_get_contents() and file was returning me an empty string "" or array() whereas the file was existing and the contents not empty.<br><br>In fact, i was locking file when writing it (file_put_contents third arg) but not testing if file was locked when reading it (and the file was accessed a lot).<br><br>So, please pay attention that file_get_contents(), file() and maybe others php files functions are going to return empty data like if the contents of the file was an empty string.<br><br>To avoid this problem, you have to set a LOCK_SH on your file before reading it (and then waiting if locked).<br><br>Something like this :<br><br>

```
<?php
public static function getContents($path, $waitIfLocked = true) {
    if(!file_exists($path)) {
        throw new Exception(&apos;File "&apos;.$path.&apos;" does not exists&apos;);
    }
    else {
        $fo = fopen($path, &apos;r&apos;);
        $locked = flock($fo, LOCK_SH, $waitIfLocked);
        
        if(!$locked) {
            return false;
        }
        else {
            $cts = file_get_contents($path);
            
            flock($fo, LOCK_UN);
            fclose($fo);
            
            return $cts;
        }
    }
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
<br><br>Then launch file.php and switch to file2.php during the 10 seconds and see the difference before/after  

#

[Official documentation page](https://www.php.net/manual/en/function.flock.php)

**[To root](/README.md)**