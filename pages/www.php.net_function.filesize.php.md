# filesize



Extremely simple function to get human filesize.<br>

```
<?php
function human_filesize($bytes, $decimals = 2) {
  $sz = 'BKMGTP';
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
            0 => array(
                "UNIT" => "TB",
                "VALUE" => pow(1024, 4)
            ),
            1 => array(
                "UNIT" => "GB",
                "VALUE" => pow(1024, 3)
            ),
            2 => array(
                "UNIT" => "MB",
                "VALUE" => pow(1024, 2)
            ),
            3 => array(
                "UNIT" => "KB",
                "VALUE" => 1024
            ),
            4 => array(
                "UNIT" => "B",
                "VALUE" => 1
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
    if (!file_exists($path))
        return false;

    $size = filesize($path);
    
    if (!($file = fopen($path, 'rb')))
        return false;
    
    if ($size &gt;= 0)
    {//Check if it really is a small file (&lt; 2 GB)
        if (fseek($file, 0, SEEK_END) === 0)
        {//It really is a small file
            fclose($file);
            return $size;
        }
    }
    
    //Quickly jump the first 2 GB with fseek. After that fseek is not working on 32 bit php (it uses int internally)
    $size = PHP_INT_MAX - 1;
    if (fseek($file, PHP_INT_MAX - 1) !== 0)
    {
        fclose($file);
        return false;
    }
    
    $length = 1024 * 1024;
    while (!feof($file))
    {//Read the file until end
        $read = fread($file, $length);
        $size = bcadd($size, $length);
    }
    $size = bcsub($size, $length);
    $size = bcadd($size, strlen($read));
    
    fclose($file);
    return $size;
}?>
```
  

#

The simplest and most efficient implemention for getting remote filesize:<br><br>

```
<?php
function remote_filesize($url) {
    static $regex = '/^Content-Length: *+\K\d++$/im';
    if (!$fp = @fopen($url, 'rb')) {
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
        exec('for %I in ("'.$file.'") do @echo %~zI', $output);
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