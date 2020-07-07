# stat





On GNU/Linux you can retrieve the number of currently running processes on the machine by doing a stat for hard links on the &apos;/proc&apos; directory like so:

$ stat -c &apos;%h&apos; /proc
118

You can do the same thing in php by doing a stat on /proc and grabbing the [3] &apos;nlink&apos; - number of links in the returned array.

Here is the function I&apos;m using, it does a clearstatcache() when called more than once.



```
<?php

/**
 * Returns the number of running processes
 *
 * @link http://php.net/clearstatcache
 * @link http://php.net/stat&#xA0; Description of stat syntax.
 * @author http://www.askapache.com/php/get-number-running-proccesses.html
 * @return int
 */
function get_process_count() {
&#xA0; static $ver, $runs = 0;
&#xA0; 
&#xA0; // check if php version supports clearstatcache params, but only check once
&#xA0; if ( is_null( $ver ) )
&#xA0; &#xA0; $ver = version_compare( PHP_VERSION, &apos;5.3.0&apos;, &apos;&gt;=&apos; );
 
&#xA0; // Only call clearstatcache() if function called more than once */
&#xA0; if ( $runs++ &gt; 0 ) { // checks if $runs &gt; 0, then increments $runs by one.
&#xA0; &#xA0; 
&#xA0; &#xA0; // if php version is &gt;= 5.3.0
&#xA0; &#xA0; if ( $ver ) {
&#xA0; &#xA0; &#xA0; clearstatcache( true, &apos;/proc&apos; );
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; // if php version is &lt; 5.3.0
&#xA0; &#xA0; &#xA0; clearstatcache();
&#xA0; &#xA0; }
&#xA0; }
&#xA0; 
&#xA0; $stat = stat( &apos;/proc&apos; );
 
&#xA0; // if stat succeeds and nlink value is present return it, otherwise return 0
&#xA0; return ( ( false !== $stat &amp;&amp; isset( $stat[3] ) ) ? $stat[3] : 0 );
}

?>
```


Example #1 get_process_count() example



```
<?php
$num_procs = get_process_count();
var_dump( $num_procs );
?>
```


The above example will output:

int(118)

Which is the number of processes that were running.

  

#



This is a souped up &apos;stat&apos; function based on 
many user-submitted code snippets and 
@ http://www.askapache.com/security/chmod-stat.html 

Give it a filename, and it returns an array like stat. 



```
<?php

function alt_stat($file) {
 
 clearstatcache();
 $ss=@stat($file);
 if(!$ss) return false; //Couldnt stat file
 
 $ts=array(
&#xA0; 0140000=&gt;&apos;ssocket&apos;,
&#xA0; 0120000=&gt;&apos;llink&apos;,
&#xA0; 0100000=&gt;&apos;-file&apos;,
&#xA0; 0060000=&gt;&apos;bblock&apos;,
&#xA0; 0040000=&gt;&apos;ddir&apos;,
&#xA0; 0020000=&gt;&apos;cchar&apos;,
&#xA0; 0010000=&gt;&apos;pfifo&apos;
 );
 
 $p=$ss[&apos;mode&apos;];
 $t=decoct($ss[&apos;mode&apos;] &amp; 0170000); // File Encoding Bit
 
 $str =(array_key_exists(octdec($t),$ts))?$ts[octdec($t)]{0}:&apos;u&apos;;
 $str.=(($p&amp;0x0100)?&apos;r&apos;:&apos;-&apos;).(($p&amp;0x0080)?&apos;w&apos;:&apos;-&apos;);
 $str.=(($p&amp;0x0040)?(($p&amp;0x0800)?&apos;s&apos;:&apos;x&apos;):(($p&amp;0x0800)?&apos;S&apos;:&apos;-&apos;));
 $str.=(($p&amp;0x0020)?&apos;r&apos;:&apos;-&apos;).(($p&amp;0x0010)?&apos;w&apos;:&apos;-&apos;);
 $str.=(($p&amp;0x0008)?(($p&amp;0x0400)?&apos;s&apos;:&apos;x&apos;):(($p&amp;0x0400)?&apos;S&apos;:&apos;-&apos;));
 $str.=(($p&amp;0x0004)?&apos;r&apos;:&apos;-&apos;).(($p&amp;0x0002)?&apos;w&apos;:&apos;-&apos;);
 $str.=(($p&amp;0x0001)?(($p&amp;0x0200)?&apos;t&apos;:&apos;x&apos;):(($p&amp;0x0200)?&apos;T&apos;:&apos;-&apos;));
 
 $s=array(
 &apos;perms&apos;=&gt;array(
&#xA0; &apos;umask&apos;=&gt;sprintf(&quot;%04o&quot;,@umask()),
&#xA0; &apos;human&apos;=&gt;$str,
&#xA0; &apos;octal1&apos;=&gt;sprintf(&quot;%o&quot;, ($ss[&apos;mode&apos;] &amp; 000777)),
&#xA0; &apos;octal2&apos;=&gt;sprintf(&quot;0%o&quot;, 0777 &amp; $p),
&#xA0; &apos;decimal&apos;=&gt;sprintf(&quot;%04o&quot;, $p),
&#xA0; &apos;fileperms&apos;=&gt;@fileperms($file),
&#xA0; &apos;mode1&apos;=&gt;$p,
&#xA0; &apos;mode2&apos;=&gt;$ss[&apos;mode&apos;]),
 
 &apos;owner&apos;=&gt;array(
&#xA0; &apos;fileowner&apos;=&gt;$ss[&apos;uid&apos;],
&#xA0; &apos;filegroup&apos;=&gt;$ss[&apos;gid&apos;],
&#xA0; &apos;owner&apos;=&gt;
&#xA0; (function_exists(&apos;posix_getpwuid&apos;))?
&#xA0; @posix_getpwuid($ss[&apos;uid&apos;]):&apos;&apos;,
&#xA0; &apos;group&apos;=&gt;
&#xA0; (function_exists(&apos;posix_getgrgid&apos;))?
&#xA0; @posix_getgrgid($ss[&apos;gid&apos;]):&apos;&apos;
&#xA0; ),
 
 &apos;file&apos;=&gt;array(
&#xA0; &apos;filename&apos;=&gt;$file,
&#xA0; &apos;realpath&apos;=&gt;(@realpath($file) != $file) ? @realpath($file) : &apos;&apos;,
&#xA0; &apos;dirname&apos;=&gt;@dirname($file),
&#xA0; &apos;basename&apos;=&gt;@basename($file)
&#xA0; ),

 &apos;filetype&apos;=&gt;array(
&#xA0; &apos;type&apos;=&gt;substr($ts[octdec($t)],1),
&#xA0; &apos;type_octal&apos;=&gt;sprintf(&quot;%07o&quot;, octdec($t)),
&#xA0; &apos;is_file&apos;=&gt;@is_file($file),
&#xA0; &apos;is_dir&apos;=&gt;@is_dir($file),
&#xA0; &apos;is_link&apos;=&gt;@is_link($file),
&#xA0; &apos;is_readable&apos;=&gt; @is_readable($file),
&#xA0; &apos;is_writable&apos;=&gt; @is_writable($file)
&#xA0; ),
&#xA0; 
 &apos;device&apos;=&gt;array(
&#xA0; &apos;device&apos;=&gt;$ss[&apos;dev&apos;], //Device
&#xA0; &apos;device_number&apos;=&gt;$ss[&apos;rdev&apos;], //Device number, if device.
&#xA0; &apos;inode&apos;=&gt;$ss[&apos;ino&apos;], //File serial number
&#xA0; &apos;link_count&apos;=&gt;$ss[&apos;nlink&apos;], //link count
&#xA0; &apos;link_to&apos;=&gt;($s[&apos;type&apos;]==&apos;link&apos;) ? @readlink($file) : &apos;&apos;
&#xA0; ),
 
 &apos;size&apos;=&gt;array(
&#xA0; &apos;size&apos;=&gt;$ss[&apos;size&apos;], //Size of file, in bytes.
&#xA0; &apos;blocks&apos;=&gt;$ss[&apos;blocks&apos;], //Number 512-byte blocks allocated
&#xA0; &apos;block_size&apos;=&gt; $ss[&apos;blksize&apos;] //Optimal block size for I/O.
&#xA0; ), 
 
 &apos;time&apos;=&gt;array(
&#xA0; &apos;mtime&apos;=&gt;$ss[&apos;mtime&apos;], //Time of last modification
&#xA0; &apos;atime&apos;=&gt;$ss[&apos;atime&apos;], //Time of last access.
&#xA0; &apos;ctime&apos;=&gt;$ss[&apos;ctime&apos;], //Time of last status change
&#xA0; &apos;accessed&apos;=&gt;@date(&apos;Y M D H:i:s&apos;,$ss[&apos;atime&apos;]),
&#xA0; &apos;modified&apos;=&gt;@date(&apos;Y M D H:i:s&apos;,$ss[&apos;mtime&apos;]),
&#xA0; &apos;created&apos;=&gt;@date(&apos;Y M D H:i:s&apos;,$ss[&apos;ctime&apos;])
&#xA0; ),
 );
 
 clearstatcache();
 return $s;
}

?>
```


|=---------[ Example Output ]

Array(
[perms] =&gt; Array
&#xA0; (
&#xA0; [umask] =&gt; 0022
&#xA0; [human] =&gt; -rw-r--r--
&#xA0; [octal1] =&gt; 644
&#xA0; [octal2] =&gt; 0644
&#xA0; [decimal] =&gt; 100644
&#xA0; [fileperms] =&gt; 33188
&#xA0; [mode1] =&gt; 33188
&#xA0; [mode2] =&gt; 33188
&#xA0; )
 
[filetype] =&gt; Array
&#xA0; (
&#xA0; [type] =&gt; file
&#xA0; [type_octal] =&gt; 0100000
&#xA0; [is_file] =&gt; 1
&#xA0; [is_dir] =&gt;
&#xA0; [is_link] =&gt;
&#xA0; [is_readable] =&gt; 1
&#xA0; [is_writable] =&gt; 1
&#xA0; )
 
[owner] =&gt; Array
&#xA0; (
&#xA0; [fileowner] =&gt; 035483
&#xA0; [filegroup] =&gt; 23472
&#xA0; [owner_name] =&gt; askapache
&#xA0; [group_name] =&gt; grp22558
&#xA0; )
 
[file] =&gt; Array
&#xA0; (
&#xA0; [filename] =&gt; /home/askapache/askapache-stat/htdocs/ok/g.php
&#xA0; [realpath] =&gt;
&#xA0; [dirname] =&gt; /home/askapache/askapache-stat/htdocs/ok
&#xA0; [basename] =&gt; g.php
&#xA0; )
 
[device] =&gt; Array
&#xA0; (
&#xA0; [device] =&gt; 25
&#xA0; [device_number] =&gt; 0
&#xA0; [inode] =&gt; 92455020
&#xA0; [link_count] =&gt; 1
&#xA0; [link_to] =&gt;
&#xA0; )
 
[size] =&gt; Array
&#xA0; (
&#xA0; [size] =&gt; 2652
&#xA0; [blocks] =&gt; 8
&#xA0; [block_size] =&gt; 8192
&#xA0; )
 
[time] =&gt; Array
&#xA0; (
&#xA0; [mtime] =&gt; 1227685253
&#xA0; [atime] =&gt; 1227685138
&#xA0; [ctime] =&gt; 1227685253
&#xA0; [accessed] =&gt; 2008 Nov Tue 23:38:58
&#xA0; [modified] =&gt; 2008 Nov Tue 23:40:53
&#xA0; [created] =&gt; 2008 Nov Tue 23:40:53
&#xA0; )
)

  

#

[Official documentation page](https://www.php.net/manual/en/function.stat.php)

**[To root](/README.md)**