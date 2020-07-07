# fseek





JUST TO QUOTE AND POINT THIS OUT:

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
3. if you&apos;re using fseek() to write data to a file, remember to open the file in &quot;r+&quot; 
mode, example:

&#xA0; $fp=fopen($filename,&quot;r+&quot;);

DON&apos;T open the file in mode &quot;a&quot; (for append), because it puts
 the file pointer at the end of the file and doesn&apos;t let you 
fseek earlier positions in the file (it didn&apos;t for me!). Also, 
don&apos;t open the file in mode &quot;w&quot; -- although this puts you at 
the beginning of the file -- because it wipes out all data in 
the file. 

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

Took me half a day to figure :/

  

#



The official docs indicate that not all streams are seekable.
You can try to seek anyway and handle failure:



```
<?php
if (fseek($stream, $offset, SEEK_CUR) === -1) {
&#xA0; // whatever
} 
?>
```


Or, you can use the stream_get_meta_data function:
http://php.net/stream_get_meta_data



```
<?php 
function fseekable($stream) {
&#xA0; $meta = stream_get_meta_data($stream);
&#xA0; return $meta[&apos;seekable&apos;];
}
?>
```



  

#



I want to give my contribution about the &quot;read last lines from a file&quot; topic. I&apos;ve done some researches (starting from here, really) and run many tests for different algorithms and scenarios, and came up with this:

What is the best way in PHP to read last lines from a file?
http://stackoverflow.com/a/15025877/995958

In that mini-article I tried to analyze all different methods and their performance over different files.

Hope it helps.

  

#



Write Dummy File 4GB in Php 32bits (X86)
if you want write more GB File (&gt;4GB), use Php(X64) .
this file is created in 0.0041329860687256 second

CreatFileDummy(&apos;data_test.txt&apos;,4294967296);

FUNCTION CreatFileDummy($file_name,$size) {&#xA0; &#xA0; 
// 32bits 4&#xA0;294&#xA0;967&#xA0;296 bytes MAX Size 
&#xA0; &#xA0; $f = fopen($file_name, &apos;wb&apos;);
&#xA0; &#xA0; if($size &gt;= 1000000000)&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $z = ($size / 1000000000);&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if (is_float($z))&#xA0; { 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $z = round($z,0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($f, ( $size - ($z * 1000000000) -1 ), SEEK_END);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($f, &quot;\0&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; while(--$z &gt; -1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($f, 999999999, SEEK_END);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($f, &quot;\0&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; } 
&#xA0; &#xA0; else {
&#xA0; &#xA0; &#xA0; &#xA0; fseek($f, $size - 1, SEEK_END);
&#xA0; &#xA0; &#xA0; &#xA0; fwrite($f, &quot;\0&quot;);
&#xA0; &#xA0; }
&#xA0; &#xA0; fclose($f);

Return true;
}

Synx

  

#

[Official documentation page](https://www.php.net/manual/en/function.fseek.php)

**[To root](/README.md)**