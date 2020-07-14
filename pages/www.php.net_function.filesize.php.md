# filesize



Extremely simple function to get human filesize.<br>

```
<?php
function human_filesize($bytes, $decimals = 2) {
  $sz = &apos;BKMGTP&apos;;
  $factor = floor((strlen($bytes) - 1) / 3);
  return sprintf("%.{$decimals}f", $bytes / pow(1024, $factor)) . @$sz[$factor];
}
?>
```
  

#

if you recently appended something to file, and closed it then this method will not show appended data:<br>

```
<?php
// get contents of a file into a string
$filename = "/usr/local/something.txt";
$handle = fopen($filename, "r");
$contents = fread($handle, filesize($filename));
fclose($handle);
?>
```
<br>You should insert a call to clearstatcache() before calling filesize()<br>I&apos;ve spent two hours to find that =/  

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
    $bytes = floatval($bytes);
        $arBytes = array(
            0 =&gt; array(
                "UNIT" =&gt; "TB",
                "VALUE" =&gt; pow(1024, 4)
            ),
            1 =&gt; array(
                "UNIT" =&gt; "GB",
                "VALUE" =&gt; pow(1024, 3)
            ),
            2 =&gt; array(
                "UNIT" =&gt; "MB",
                "VALUE" =&gt; pow(1024, 2)
            ),
            3 =&gt; array(
                "UNIT" =&gt; "KB",
                "VALUE" =&gt; 1024
            ),
            4 =&gt; array(
                "UNIT" =&gt; "B",
                "VALUE" =&gt; 1
            ),
        );

    foreach($arBytes as $arItem)
    {
        if($bytes &gt;= $arItem["VALUE"])
        {
            $result = $bytes / $arItem["VALUE"];
            $result = str_replace(".", "," , strval(round($result, 2)))." ".$arItem["UNIT"];
            break;
        }
    }
    return $result;
}

?>
```
  

#



```
<?php<br>/**<br> * Return file size (even for file &gt; 2 Gb)<br> * For file size over PHP_INT_MAX (2 147 483 647), PHP filesize function loops from -PHP_INT_MAX to PHP_INT_MAX.<br> *<br> * @param string $path Path of the file<br> * @return mixed File size or false if error<br> */<br>function realFileSize($path)<br>{<br>    if (!file_exists($path))<br>        return false;<br><br>    $size = filesize($path);<br>    <br>    if (!($file = fopen($path, &apos;rb&apos;)))<br>        return false;<br>    <br>    if ($size &gt;= 0)<br>    {//Check if it really is a small file (&lt; 2 GB)<br>        if (fseek($file, 0, SEEK_END) === 0)<br>        {//It really is a small file<br>            fclose($file);<br>            return $size;<br>        }<br>    }<br>    <br>    //Quickly jump the first 2 GB with fseek. After that fseek is not working on 32 bit php (it uses int internally)<br>    $size = PHP_INT_MAX - 1;<br>    if (fseek($file, PHP_INT_MAX - 1) !== 0)<br>    {<br>        fclose($file);<br>        return false;<br>    }<br>    <br>    $length = 1024 * 1024;<br>    while (!feof($file))<br>    {//Read the file until end<br>        $read = fread($file, $length);<br>        $size = bcadd($size, $length);<br>    }<br>    $size = bcsub($size, $length);<br>    $size = bcadd($size, strlen($read));<br>    <br>    fclose($file);<br>    return $size;<br>}  

#

The simplest and most efficient implemention for getting remote filesize:<br><br>

```
<?php
function remote_filesize($url) {
    static $regex = &apos;/^Content-Length: *+\K\d++$/im&apos;;
    if (!$fp = @fopen($url, &apos;rb&apos;)) {
        return false;
    }
    if (
        isset($http_response_header) &amp;&amp;
        preg_match($regex, implode("\n", $http_response_header), $matches)
    ) {
        return (int)$matches[0];
    }
    return strlen(stream_get_contents($fp));
}
?>
```
  

#

Here is my super fast method of getting &gt;2GB files to output the correct byte size on any version of windows works with both 32Bit and 64Bit.<br><br>

```
<?php
function find_filesize($file)
{
    if(substr(PHP_OS, 0, 3) == "WIN")
    {
        exec(&apos;for %I in ("&apos;.$file.&apos;") do @echo %~zI&apos;, $output);
        $return = $output[0];
    }
    else
    {
        $return = filesize($file);
    }
    return $return;
}

//Usage : find_filesize("path");
//Example :
echo "File size is : ".find_filesize("D:\Server\movie.mp4")."";
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.filesize.php)

**[To root](/README.md)**