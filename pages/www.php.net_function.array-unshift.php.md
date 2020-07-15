# array_unshift



You can preserve keys and unshift an array with numerical indexes in a really simple way if you&apos;ll do the following:<br><br>

```
<?php
$someArray=array(224=>'someword1', 228=>'someword2', 102=>'someword3', 544=>'someword3',95=>'someword4');

$someArray=array(100=>'Test Element 1 ',255=>'Test Element 2')+$someArray;
?>
```
<br><br>now the array looks as follows:<br><br>array(<br>100=&gt;&apos;Test Element 1 &apos;,<br>255=&gt;&apos;Test Element 2&apos;<br>224=&gt;&apos;someword1&apos;,<br>228=&gt;&apos;someword2&apos;,<br>102=&gt;&apos;someword3&apos;,<br>544=&gt;&apos;someword3&apos;,<br>95=&gt;&apos;someword4&apos;<br>);  

#

array_merge() will also reindex (see array_merge() manual entry), but the &apos;+&apos; operator won&apos;t, so...<br><br>

```
<?php
$arrayone=array("newkey"=>"newvalue") + $arrayone;
?>
```
<br><br>does the job.  

#

Sahn&apos;s example almost works but has a small error. Try it like this if you need to prepend something to the array without the keys being reindexed and/or need to prepend a key value pair, you can use this short function: <br><br>

```
<?php 
function array_unshift_assoc(&amp;$arr, $key, $val) 
{ 
    $arr = array_reverse($arr, true); 
    $arr[$key] = $val; 
    return = array_reverse($arr, true); 
} 
?>
```
  

#

Anonymous&apos; associative version wasn&apos;t working for me, but it did with this small tweak:<br><br>function array_unshift_assoc(&amp;$arr, $key, $val) <br>{ <br>    $arr = array_reverse($arr, true); <br>    $arr[$key] = $val; <br>    $arr = array_reverse($arr, true); <br>    return $arr;<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.array-unshift.php)

**[To root](/README.md)**