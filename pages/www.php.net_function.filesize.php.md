# filesize





Extremely simple function to get human filesize.


```
<?php
function human_filesize($bytes, $decimals = 2) {
&#xA0; $sz = &apos;BKMGTP&apos;;
&#xA0; $factor = floor((strlen($bytes) - 1) / 3);
&#xA0; return sprintf(&quot;%.{$decimals}f&quot;, $bytes / pow(1024, $factor)) . @$sz[$factor];
}
?>
```



  

#



if you recently appended something to file, and closed it then this method will not show appended data:


```
<?php
// get contents of a file into a string
$filename = &quot;/usr/local/something.txt&quot;;
$handle = fopen($filename, &quot;r&quot;);
$contents = fread($handle, filesize($filename));
fclose($handle);
?>
```

You should insert a call to clearstatcache() before calling filesize()
I&apos;ve spent two hours to find that =/

  

#





```
<?php
/** 
* Converts bytes into human readable file size. 
* 
* @param string $bytes 
* @return string human readable file size (2,87 &#x41C;&#x431;)
* @author Mogilev Arseny 
*/ 
function FileSizeConvert($bytes)
{
&#xA0; &#xA0; $bytes = floatval($bytes);
&#xA0; &#xA0; &#xA0; &#xA0; $arBytes = array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0 =&gt; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;UNIT&quot; =&gt; &quot;TB&quot;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;VALUE&quot; =&gt; pow(1024, 4)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 1 =&gt; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;UNIT&quot; =&gt; &quot;GB&quot;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;VALUE&quot; =&gt; pow(1024, 3)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 2 =&gt; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;UNIT&quot; =&gt; &quot;MB&quot;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;VALUE&quot; =&gt; pow(1024, 2)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 3 =&gt; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;UNIT&quot; =&gt; &quot;KB&quot;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;VALUE&quot; =&gt; 1024
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 4 =&gt; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;UNIT&quot; =&gt; &quot;B&quot;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;VALUE&quot; =&gt; 1
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; &#xA0; &#xA0; );

&#xA0; &#xA0; foreach($arBytes as $arItem)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if($bytes &gt;= $arItem[&quot;VALUE&quot;])
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = $bytes / $arItem[&quot;VALUE&quot;];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = str_replace(&quot;.&quot;, &quot;,&quot; , strval(round($result, 2))).&quot; &quot;.$arItem[&quot;UNIT&quot;];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; return $result;
}

?>
```



  

#





```
<?php
/**
 * Return file size (even for file &gt; 2 Gb)
 * For file size over PHP_INT_MAX (2 147 483 647), PHP filesize function loops from -PHP_INT_MAX to PHP_INT_MAX.
 *
 * @param string $path Path of the file
 * @return mixed File size or false if error
 */
function realFileSize($path)
{
&#xA0; &#xA0; if (!file_exists($path))
&#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; $size = filesize($path);
&#xA0; &#xA0; 
&#xA0; &#xA0; if (!($file = fopen($path, &apos;rb&apos;)))
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; 
&#xA0; &#xA0; if ($size &gt;= 0)
&#xA0; &#xA0; {//Check if it really is a small file (&lt; 2 GB)
&#xA0; &#xA0; &#xA0; &#xA0; if (fseek($file, 0, SEEK_END) === 0)
&#xA0; &#xA0; &#xA0; &#xA0; {//It really is a small file
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($file);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $size;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; //Quickly jump the first 2 GB with fseek. After that fseek is not working on 32 bit php (it uses int internally)
&#xA0; &#xA0; $size = PHP_INT_MAX - 1;
&#xA0; &#xA0; if (fseek($file, PHP_INT_MAX - 1) !== 0)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; fclose($file);
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; $length = 1024 * 1024;
&#xA0; &#xA0; while (!feof($file))
&#xA0; &#xA0; {//Read the file until end
&#xA0; &#xA0; &#xA0; &#xA0; $read = fread($file, $length);
&#xA0; &#xA0; &#xA0; &#xA0; $size = bcadd($size, $length);
&#xA0; &#xA0; }
&#xA0; &#xA0; $size = bcsub($size, $length);
&#xA0; &#xA0; $size = bcadd($size, strlen($read));
&#xA0; &#xA0; 
&#xA0; &#xA0; fclose($file);
&#xA0; &#xA0; return $size;
}


  

#



The simplest and most efficient implemention for getting remote filesize:



```
<?php
function remote_filesize($url) {
&#xA0; &#xA0; static $regex = &apos;/^Content-Length: *+\K\d++$/im&apos;;
&#xA0; &#xA0; if (!$fp = @fopen($url, &apos;rb&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; if (
&#xA0; &#xA0; &#xA0; &#xA0; isset($http_response_header) &amp;&amp;
&#xA0; &#xA0; &#xA0; &#xA0; preg_match($regex, implode(&quot;\n&quot;, $http_response_header), $matches)
&#xA0; &#xA0; ) {
&#xA0; &#xA0; &#xA0; &#xA0; return (int)$matches[0];
&#xA0; &#xA0; }
&#xA0; &#xA0; return strlen(stream_get_contents($fp));
}
?>
```



  

#



Here is my super fast method of getting &gt;2GB files to output the correct byte size on any version of windows works with both 32Bit and 64Bit.



```
<?php
function find_filesize($file)
{
&#xA0; &#xA0; if(substr(PHP_OS, 0, 3) == &quot;WIN&quot;)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; exec(&apos;for %I in (&quot;&apos;.$file.&apos;&quot;) do @echo %~zI&apos;, $output);
&#xA0; &#xA0; &#xA0; &#xA0; $return = $output[0];
&#xA0; &#xA0; }
&#xA0; &#xA0; else
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $return = filesize($file);
&#xA0; &#xA0; }
&#xA0; &#xA0; return $return;
}

//Usage : find_filesize(&quot;path&quot;);
//Example :
echo &quot;File size is : &quot;.find_filesize(&quot;D:\Server\movie.mp4&quot;).&quot;&quot;;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.filesize.php)

**[To root](/README.md)**