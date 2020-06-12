# flock




<div class="phpcode"><span class="html">
The supplied documentation is vague, ambiguous and lacking, and the user comments contain erroneous information! The flock function follows the semantics of the Unix system call bearing the same name. Flock utilizes ADVISORY locking only; that is, other processes may ignore the lock completely; it only affects those that call the flock call.<br><br>LOCK_SH means SHARED LOCK. Any number of processes MAY HAVE A SHARED LOCK simultaneously. It is commonly called a reader lock.<br><br>LOCK_EX means EXCLUSIVE LOCK. Only a single process may possess an exclusive lock to a given file at a time.<br><br>If the file has been LOCKED with LOCK_SH in another process, flock with LOCK_SH will SUCCEED. flock with LOCK_EX will BLOCK UNTIL ALL READER LOCKS HAVE BEEN RELEASED.<br><br>If the file has been locked with LOCK_EX in another process, the CALL WILL BLOCK UNTIL ALL OTHER LOCKS have been released.<br><br>If however, you call flock on a file on which you possess the lock, it will try to change it. So: flock(LOCK_EX) followed by flock(LOCK_SH) will get you a SHARED lock, not &quot;read-write&quot; lock.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Simple Helper for lock files creation<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">FileLocker </span><span class="keyword">{<br>&#xA0; &#xA0; protected static </span><span class="default">$loc_files </span><span class="keyword">= array();<br><br>&#xA0; &#xA0; public static function </span><span class="default">lockFile</span><span class="keyword">(</span><span class="default">$file_name</span><span class="keyword">, </span><span class="default">$wait </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$loc_file </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$file_name</span><span class="keyword">, </span><span class="string">&apos;c&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( !</span><span class="default">$loc_file </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \</span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Can\&apos;t create lock file!&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">$wait </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$lock </span><span class="keyword">= </span><span class="default">flock</span><span class="keyword">(</span><span class="default">$loc_file</span><span class="keyword">, </span><span class="default">LOCK_EX</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$lock </span><span class="keyword">= </span><span class="default">flock</span><span class="keyword">(</span><span class="default">$loc_file</span><span class="keyword">, </span><span class="default">LOCK_EX </span><span class="keyword">| </span><span class="default">LOCK_NB</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">$lock </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$loc_files</span><span class="keyword">[</span><span class="default">$file_name</span><span class="keyword">] = </span><span class="default">$loc_file</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fprintf</span><span class="keyword">(</span><span class="default">$loc_file</span><span class="keyword">, </span><span class="string">&quot;%s\n&quot;</span><span class="keyword">, </span><span class="default">getmypid</span><span class="keyword">());<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$loc_file</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } else if ( </span><span class="default">$wait </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \</span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Can\&apos;t lock file!&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public static function </span><span class="default">unlockFile</span><span class="keyword">(</span><span class="default">$file_name</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">self</span><span class="keyword">::</span><span class="default">$loc_files</span><span class="keyword">[</span><span class="default">$file_name</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; @</span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$file_name</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">self</span><span class="keyword">::</span><span class="default">$loc_files</span><span class="keyword">[</span><span class="default">$file_name</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br><br>} <br><br>if ( !</span><span class="default">FileLocker</span><span class="keyword">::</span><span class="default">lockFile</span><span class="keyword">(</span><span class="string">&apos;/tmp/1.lock&apos;</span><span class="keyword">) ) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Can&apos;t lock file\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; die();<br>}<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">);<br></span><span class="default">FileLocker</span><span class="keyword">::</span><span class="default">unlockFile</span><span class="keyword">(</span><span class="string">&apos;/tmp/1.lock&apos;</span><span class="keyword">);<br>echo </span><span class="string">&quot;All Ok\n&quot;</span><span class="keyword">;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Regarding the change in PHP 5.3.2 with locked files:<br><br>Without having studied the PHP source code in detail, the situation appears to be as follows when the PHP function fclose() is called:<br><br>Before 5.3.2 PHP would check if the file was locked, then release the lock, and then close the file.<br><br>From 5.3.2 PHP just closes the file.<br><br>But note, that the operating system releases the lock automatically when the file is closed. Therefore a call to fclose() STILL releases the lock (this is tested with PHP 5.3.2, Linux, x64).</span>
</div>
  

#


<div class="phpcode"><span class="html">
I just spent some time (again) to understand why a reading with file_get_contents() and file was returning me an empty string &quot;&quot; or array() whereas the file was existing and the contents not empty.
<br>
<br>In fact, i was locking file when writing it (file_put_contents third arg) but not testing if file was locked when reading it (and the file was accessed a lot).
<br>
<br>So, please pay attention that file_get_contents(), file() and maybe others php files functions are going to return empty data like if the contents of the file was an empty string.
<br>
<br>To avoid this problem, you have to set a LOCK_SH on your file before reading it (and then waiting if locked).
<br>
<br>Something like this :
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">public static function </span><span class="default">getContents</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="default">$waitIfLocked </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">) {
<br>&#xA0; &#xA0; if(!</span><span class="default">file_exists</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;File &quot;&apos;</span><span class="keyword">.</span><span class="default">$path</span><span class="keyword">.</span><span class="string">&apos;&quot; does not exists&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fo </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="string">&apos;r&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$locked </span><span class="keyword">= </span><span class="default">flock</span><span class="keyword">(</span><span class="default">$fo</span><span class="keyword">, </span><span class="default">LOCK_SH</span><span class="keyword">, </span><span class="default">$waitIfLocked</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$locked</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cts </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">flock</span><span class="keyword">(</span><span class="default">$fo</span><span class="keyword">, </span><span class="default">LOCK_UN</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fo</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$cts</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Code to test by yourself :
<br>
<br>abc.txt :
<br>someText
<br>
<br>file.php :
<br><span class="default">&lt;?php
<br>$fo </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;abc.txt&apos;</span><span class="keyword">, </span><span class="string">&apos;r+&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">flock</span><span class="keyword">(</span><span class="default">$fo</span><span class="keyword">, </span><span class="default">LOCK_EX</span><span class="keyword">);
<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">);
<br></span><span class="default">flock</span><span class="keyword">(</span><span class="default">$fo</span><span class="keyword">, </span><span class="default">LOCK_UN</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>file2.php : 
<br><span class="default">&lt;?php
<br>var_dump</span><span class="keyword">(</span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;abc.txt&apos;</span><span class="keyword">));
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">file</span><span class="keyword">(</span><span class="string">&apos;abc.txt&apos;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>Then launch file.php and switch to file2.php during the 10 seconds and see the difference before/after</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.flock.php)

**[⬆ to root](/)**