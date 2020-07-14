# fseek



JUST TO QUOTE AND POINT THIS OUT:<br><br>!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!<br>3. if you&apos;re using fseek() to write data to a file, remember to open the file in "r+" <br>mode, example:<br><br>  $fp=fopen($filename,"r+");<br><br>DON&apos;T open the file in mode "a" (for append), because it puts<br> the file pointer at the end of the file and doesn&apos;t let you <br>fseek earlier positions in the file (it didn&apos;t for me!). Also, <br>don&apos;t open the file in mode "w" -- although this puts you at <br>the beginning of the file -- because it wipes out all data in <br>the file. <br><br>!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!<br><br>Took me half a day to figure :/  

#

The official docs indicate that not all streams are seekable.<br>You can try to seek anyway and handle failure:<br><br>

```
<?php
if (fseek($stream, $offset, SEEK_CUR) === -1) {
  // whatever
} 
?>
```


Or, you can use the stream_get_meta_data function:
http://php.net/stream_get_meta_data



```
<?php 
function fseekable($stream) {
  $meta = stream_get_meta_data($stream);
  return $meta[&apos;seekable&apos;];
}
?>
```
  

#

I want to give my contribution about the "read last lines from a file" topic. I&apos;ve done some researches (starting from here, really) and run many tests for different algorithms and scenarios, and came up with this:<br><br>What is the best way in PHP to read last lines from a file?<br>http://stackoverflow.com/a/15025877/995958<br><br>In that mini-article I tried to analyze all different methods and their performance over different files.<br><br>Hope it helps.  

#

Write Dummy File 4GB in Php 32bits (X86)<br>if you want write more GB File (&gt;4GB), use Php(X64) .<br>this file is created in 0.0041329860687256 second<br><br>CreatFileDummy(&apos;data_test.txt&apos;,4294967296);<br><br>FUNCTION CreatFileDummy($file_name,$size) {    <br>// 32bits 4 294 967 296 bytes MAX Size <br>    $f = fopen($file_name, &apos;wb&apos;);<br>    if($size &gt;= 1000000000)  {<br>        $z = ($size / 1000000000);        <br>        if (is_float($z))  { <br>            $z = round($z,0);<br>            fseek($f, ( $size - ($z * 1000000000) -1 ), SEEK_END);<br>            fwrite($f, "\0");<br>        }        <br>        while(--$z &gt; -1) {<br>            fseek($f, 999999999, SEEK_END);<br>            fwrite($f, "\0");<br>        }<br>    } <br>    else {<br>        fseek($f, $size - 1, SEEK_END);<br>        fwrite($f, "\0");<br>    }<br>    fclose($f);<br><br>Return true;<br>}<br><br>Synx  

#

[Official documentation page](https://www.php.net/manual/en/function.fseek.php)

**[To root](/README.md)**