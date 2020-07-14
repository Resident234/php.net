# stat



On GNU/Linux you can retrieve the number of currently running processes on the machine by doing a stat for hard links on the &apos;/proc&apos; directory like so:<br><br>$ stat -c &apos;%h&apos; /proc<br>118<br><br>You can do the same thing in php by doing a stat on /proc and grabbing the [3] &apos;nlink&apos; - number of links in the returned array.<br><br>Here is the function I&apos;m using, it does a clearstatcache() when called more than once.<br><br>

```
<?php

/**
 * Returns the number of running processes
 *
 * @link http://php.net/clearstatcache
 * @link http://php.net/stat  Description of stat syntax.
 * @author http://www.askapache.com/php/get-number-running-proccesses.html
 * @return int
 */
function get_process_count() {
  static $ver, $runs = 0;
  
  // check if php version supports clearstatcache params, but only check once
  if ( is_null( $ver ) )
    $ver = version_compare( PHP_VERSION, &apos;5.3.0&apos;, &apos;&gt;=&apos; );
 
  // Only call clearstatcache() if function called more than once */
  if ( $runs++ &gt; 0 ) { // checks if $runs &gt; 0, then increments $runs by one.
    
    // if php version is &gt;= 5.3.0
    if ( $ver ) {
      clearstatcache( true, &apos;/proc&apos; );
    } else {
      // if php version is &lt; 5.3.0
      clearstatcache();
    }
  }
  
  $stat = stat( &apos;/proc&apos; );
 
  // if stat succeeds and nlink value is present return it, otherwise return 0
  return ( ( false !== $stat &amp;&amp; isset( $stat[3] ) ) ? $stat[3] : 0 );
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
<br><br>The above example will output:<br><br>int(118)<br><br>Which is the number of processes that were running.  

#

This is a souped up &apos;stat&apos; function based on <br>many user-submitted code snippets and <br>@ http://www.askapache.com/security/chmod-stat.html <br><br>Give it a filename, and it returns an array like stat. <br><br>

```
<?php

function alt_stat($file) {
 
 clearstatcache();
 $ss=@stat($file);
 if(!$ss) return false; //Couldnt stat file
 
 $ts=array(
  0140000=&gt;&apos;ssocket&apos;,
  0120000=&gt;&apos;llink&apos;,
  0100000=&gt;&apos;-file&apos;,
  0060000=&gt;&apos;bblock&apos;,
  0040000=&gt;&apos;ddir&apos;,
  0020000=&gt;&apos;cchar&apos;,
  0010000=&gt;&apos;pfifo&apos;
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
  &apos;umask&apos;=&gt;sprintf("%04o",@umask()),
  &apos;human&apos;=&gt;$str,
  &apos;octal1&apos;=&gt;sprintf("%o", ($ss[&apos;mode&apos;] &amp; 000777)),
  &apos;octal2&apos;=&gt;sprintf("0%o", 0777 &amp; $p),
  &apos;decimal&apos;=&gt;sprintf("%04o", $p),
  &apos;fileperms&apos;=&gt;@fileperms($file),
  &apos;mode1&apos;=&gt;$p,
  &apos;mode2&apos;=&gt;$ss[&apos;mode&apos;]),
 
 &apos;owner&apos;=&gt;array(
  &apos;fileowner&apos;=&gt;$ss[&apos;uid&apos;],
  &apos;filegroup&apos;=&gt;$ss[&apos;gid&apos;],
  &apos;owner&apos;=&gt;
  (function_exists(&apos;posix_getpwuid&apos;))?
  @posix_getpwuid($ss[&apos;uid&apos;]):&apos;&apos;,
  &apos;group&apos;=&gt;
  (function_exists(&apos;posix_getgrgid&apos;))?
  @posix_getgrgid($ss[&apos;gid&apos;]):&apos;&apos;
  ),
 
 &apos;file&apos;=&gt;array(
  &apos;filename&apos;=&gt;$file,
  &apos;realpath&apos;=&gt;(@realpath($file) != $file) ? @realpath($file) : &apos;&apos;,
  &apos;dirname&apos;=&gt;@dirname($file),
  &apos;basename&apos;=&gt;@basename($file)
  ),

 &apos;filetype&apos;=&gt;array(
  &apos;type&apos;=&gt;substr($ts[octdec($t)],1),
  &apos;type_octal&apos;=&gt;sprintf("%07o", octdec($t)),
  &apos;is_file&apos;=&gt;@is_file($file),
  &apos;is_dir&apos;=&gt;@is_dir($file),
  &apos;is_link&apos;=&gt;@is_link($file),
  &apos;is_readable&apos;=&gt; @is_readable($file),
  &apos;is_writable&apos;=&gt; @is_writable($file)
  ),
  
 &apos;device&apos;=&gt;array(
  &apos;device&apos;=&gt;$ss[&apos;dev&apos;], //Device
  &apos;device_number&apos;=&gt;$ss[&apos;rdev&apos;], //Device number, if device.
  &apos;inode&apos;=&gt;$ss[&apos;ino&apos;], //File serial number
  &apos;link_count&apos;=&gt;$ss[&apos;nlink&apos;], //link count
  &apos;link_to&apos;=&gt;($s[&apos;type&apos;]==&apos;link&apos;) ? @readlink($file) : &apos;&apos;
  ),
 
 &apos;size&apos;=&gt;array(
  &apos;size&apos;=&gt;$ss[&apos;size&apos;], //Size of file, in bytes.
  &apos;blocks&apos;=&gt;$ss[&apos;blocks&apos;], //Number 512-byte blocks allocated
  &apos;block_size&apos;=&gt; $ss[&apos;blksize&apos;] //Optimal block size for I/O.
  ), 
 
 &apos;time&apos;=&gt;array(
  &apos;mtime&apos;=&gt;$ss[&apos;mtime&apos;], //Time of last modification
  &apos;atime&apos;=&gt;$ss[&apos;atime&apos;], //Time of last access.
  &apos;ctime&apos;=&gt;$ss[&apos;ctime&apos;], //Time of last status change
  &apos;accessed&apos;=&gt;@date(&apos;Y M D H:i:s&apos;,$ss[&apos;atime&apos;]),
  &apos;modified&apos;=&gt;@date(&apos;Y M D H:i:s&apos;,$ss[&apos;mtime&apos;]),
  &apos;created&apos;=&gt;@date(&apos;Y M D H:i:s&apos;,$ss[&apos;ctime&apos;])
  ),
 );
 
 clearstatcache();
 return $s;
}

?>
```


|=---------[ Example Output ]

Array(
[perms] =&gt; Array
  (
  [umask] =&gt; 0022
  [human] =&gt; -rw-r--r--
  [octal1] =&gt; 644
  [octal2] =&gt; 0644
  [decimal] =&gt; 100644
  [fileperms] =&gt; 33188
  [mode1] =&gt; 33188
  [mode2] =&gt; 33188
  )
 
[filetype] =&gt; Array
  (
  [type] =&gt; file
  [type_octal] =&gt; 0100000
  [is_file] =&gt; 1
  [is_dir] =&gt;
  [is_link] =&gt;
  [is_readable] =&gt; 1
  [is_writable] =&gt; 1
  )
 
[owner] =&gt; Array
  (
  [fileowner] =&gt; 035483
  [filegroup] =&gt; 23472
  [owner_name] =&gt; askapache
  [group_name] =&gt; grp22558
  )
 
[file] =&gt; Array
  (
  [filename] =&gt; /home/askapache/askapache-stat/htdocs/ok/g.php
  [realpath] =&gt;
  [dirname] =&gt; /home/askapache/askapache-stat/htdocs/ok
  [basename] =&gt; g.php<br>  )<br> <br>[device] =&gt; Array<br>  (<br>  [device] =&gt; 25<br>  [device_number] =&gt; 0<br>  [inode] =&gt; 92455020<br>  [link_count] =&gt; 1<br>  [link_to] =&gt;<br>  )<br> <br>[size] =&gt; Array<br>  (<br>  [size] =&gt; 2652<br>  [blocks] =&gt; 8<br>  [block_size] =&gt; 8192<br>  )<br> <br>[time] =&gt; Array<br>  (<br>  [mtime] =&gt; 1227685253<br>  [atime] =&gt; 1227685138<br>  [ctime] =&gt; 1227685253<br>  [accessed] =&gt; 2008 Nov Tue 23:38:58<br>  [modified] =&gt; 2008 Nov Tue 23:40:53<br>  [created] =&gt; 2008 Nov Tue 23:40:53<br>  )<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.stat.php)

**[To root](/README.md)**