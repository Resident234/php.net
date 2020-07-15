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
    $ver = version_compare( PHP_VERSION, '5.3.0', '>=' );
 
  // Only call clearstatcache() if function called more than once */
  if ( $runs++ > 0 ) { // checks if $runs > 0, then increments $runs by one.
    
    // if php version is >= 5.3.0
    if ( $ver ) {
      clearstatcache( true, '/proc' );
    } else {
      // if php version is < 5.3.0
      clearstatcache();
    }
  }
  
  $stat = stat( '/proc' );
 
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
  0140000=>'ssocket',
  0120000=>'llink',
  0100000=>'-file',
  0060000=>'bblock',
  0040000=>'ddir',
  0020000=>'cchar',
  0010000=>'pfifo'
 );
 
 $p=$ss['mode'];
 $t=decoct($ss['mode'] &amp; 0170000); // File Encoding Bit
 
 $str =(array_key_exists(octdec($t),$ts))?$ts[octdec($t)]{0}:'u';
 $str.=(($p&amp;0x0100)?'r':'-').(($p&amp;0x0080)?'w':'-');
 $str.=(($p&amp;0x0040)?(($p&amp;0x0800)?'s':'x'):(($p&amp;0x0800)?'S':'-'));
 $str.=(($p&amp;0x0020)?'r':'-').(($p&amp;0x0010)?'w':'-');
 $str.=(($p&amp;0x0008)?(($p&amp;0x0400)?'s':'x'):(($p&amp;0x0400)?'S':'-'));
 $str.=(($p&amp;0x0004)?'r':'-').(($p&amp;0x0002)?'w':'-');
 $str.=(($p&amp;0x0001)?(($p&amp;0x0200)?'t':'x'):(($p&amp;0x0200)?'T':'-'));
 
 $s=array(
 'perms'=>array(
  'umask'=>sprintf("%04o",@umask()),
  'human'=>$str,
  'octal1'=>sprintf("%o", ($ss['mode'] &amp; 000777)),
  'octal2'=>sprintf("0%o", 0777 &amp; $p),
  'decimal'=>sprintf("%04o", $p),
  'fileperms'=>@fileperms($file),
  'mode1'=>$p,
  'mode2'=>$ss['mode']),
 
 'owner'=>array(
  'fileowner'=>$ss['uid'],
  'filegroup'=>$ss['gid'],
  'owner'=>
  (function_exists('posix_getpwuid'))?
  @posix_getpwuid($ss['uid']):'',
  'group'=>
  (function_exists('posix_getgrgid'))?
  @posix_getgrgid($ss['gid']):''
  ),
 
 'file'=>array(
  'filename'=>$file,
  'realpath'=>(@realpath($file) != $file) ? @realpath($file) : '',
  'dirname'=>@dirname($file),
  'basename'=>@basename($file)
  ),

 'filetype'=>array(
  'type'=>substr($ts[octdec($t)],1),
  'type_octal'=>sprintf("%07o", octdec($t)),
  'is_file'=>@is_file($file),
  'is_dir'=>@is_dir($file),
  'is_link'=>@is_link($file),
  'is_readable'=> @is_readable($file),
  'is_writable'=> @is_writable($file)
  ),
  
 'device'=>array(
  'device'=>$ss['dev'], //Device
  'device_number'=>$ss['rdev'], //Device number, if device.
  'inode'=>$ss['ino'], //File serial number
  'link_count'=>$ss['nlink'], //link count
  'link_to'=>($s['type']=='link') ? @readlink($file) : ''
  ),
 
 'size'=>array(
  'size'=>$ss['size'], //Size of file, in bytes.
  'blocks'=>$ss['blocks'], //Number 512-byte blocks allocated
  'block_size'=> $ss['blksize'] //Optimal block size for I/O.
  ), 
 
 'time'=>array(
  'mtime'=>$ss['mtime'], //Time of last modification
  'atime'=>$ss['atime'], //Time of last access.
  'ctime'=>$ss['ctime'], //Time of last status change
  'accessed'=>@date('Y M D H:i:s',$ss['atime']),
  'modified'=>@date('Y M D H:i:s',$ss['mtime']),
  'created'=>@date('Y M D H:i:s',$ss['ctime'])
  ),
 );
 
 clearstatcache();
 return $s;
}

?>
```
<br><br>|=---------[ Example Output ]<br><br>Array(<br>[perms] =&gt; Array<br>  (<br>  [umask] =&gt; 0022<br>  [human] =&gt; -rw-r--r--<br>  [octal1] =&gt; 644<br>  [octal2] =&gt; 0644<br>  [decimal] =&gt; 100644<br>  [fileperms] =&gt; 33188<br>  [mode1] =&gt; 33188<br>  [mode2] =&gt; 33188<br>  )<br> <br>[filetype] =&gt; Array<br>  (<br>  [type] =&gt; file<br>  [type_octal] =&gt; 0100000<br>  [is_file] =&gt; 1<br>  [is_dir] =&gt;<br>  [is_link] =&gt;<br>  [is_readable] =&gt; 1<br>  [is_writable] =&gt; 1<br>  )<br> <br>[owner] =&gt; Array<br>  (<br>  [fileowner] =&gt; 035483<br>  [filegroup] =&gt; 23472<br>  [owner_name] =&gt; askapache<br>  [group_name] =&gt; grp22558<br>  )<br> <br>[file] =&gt; Array<br>  (<br>  [filename] =&gt; /home/askapache/askapache-stat/htdocs/ok/g.php<br>  [realpath] =&gt;<br>  [dirname] =&gt; /home/askapache/askapache-stat/htdocs/ok<br>  [basename] =&gt; g.php<br>  )<br> <br>[device] =&gt; Array<br>  (<br>  [device] =&gt; 25<br>  [device_number] =&gt; 0<br>  [inode] =&gt; 92455020<br>  [link_count] =&gt; 1<br>  [link_to] =&gt;<br>  )<br> <br>[size] =&gt; Array<br>  (<br>  [size] =&gt; 2652<br>  [blocks] =&gt; 8<br>  [block_size] =&gt; 8192<br>  )<br> <br>[time] =&gt; Array<br>  (<br>  [mtime] =&gt; 1227685253<br>  [atime] =&gt; 1227685138<br>  [ctime] =&gt; 1227685253<br>  [accessed] =&gt; 2008 Nov Tue 23:38:58<br>  [modified] =&gt; 2008 Nov Tue 23:40:53<br>  [created] =&gt; 2008 Nov Tue 23:40:53<br>  )<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.stat.php)

**[To root](/README.md)**