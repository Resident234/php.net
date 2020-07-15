# strtolower



strtolower(); doesn&apos;t work for polish chars<br><br>

```
<?php strtolower("m&#x104;kA"); ?>
```

will return: m&#x104;ka;

the best solution - use mb_strtolower()



```
<?php mb_strtolower("m&#x104;kA",'UTF-8'); ?>
```
<br>will return: m&#x105;ka  

#

for cyrillic and UTF 8 use  mb_convert_case<br><br>exampel<br><br>

```
<?php
$string = "&#x410;&#x432;&#x441;&#x442;&#x440;&#x430;&#x43B;&#x438;&#x44F;";
$string = mb_convert_case($string, MB_CASE_LOWER, "UTF-8");
echo $string;

//output is: &#x430;&#x432;&#x441;&#x442;&#x440;&#x430;&#x43B;&#x438;&#x44F;
?>
```
  

#

It is worth noting that <br>

```
<?php 
var_dump(strtolower(null))
?>
```
<br>returns: <br>string(0) ""  

#

the function  arraytolower will create duplicate entries since keys are case sensitive.  <br><br>

```
<?php
$array = array('test1' => 'asgAFasDAAd', 'TEST2' => 'ASddhshsDGb', 'TeSt3 '=> 'asdasda@asdadadASDASDgh');

$array = arraytolower($array);
?>
```

/*
Array
(
    [test1] => asgafasdaad
    [TEST2] => ASddhshsDGb
    [TeSt3] => asdasda@asdadadASDASDgh
    [test2] => asddhshsdgb
    [test3] => asdasda@asdadadasdasdgh
)
*/

I prefer this method



```
<?php
  function arraytolower($array, $include_leys=false) {
    
    if($include_leys) {
      foreach($array as $key => $value) {
        if(is_array($value))
          $array2[strtolower($key)] = arraytolower($value, $include_leys);
        else
          $array2[strtolower($key)] = strtolower($value);
      }
      $array = $array2;
    }
    else {
      foreach($array as $key => $value) {
        if(is_array($value))
          $array[$key] = arraytolower($value, $include_leys);
        else
          $array[$key] = strtolower($value);   
      }
    }
    
    return $array;
  } 
?>
```


which when used like this



```
<?php
$array = $array = array('test1' => 'asgAFasDAAd', 'TEST2' => 'ASddhshsDGb', 'TeSt3 '=> 'asdasda@asdadadASDASDgh');

$array1 = arraytolower($array);
$array2 = arraytolower($array,true);

print_r($array1);
print_r($array2);
?>
```
<br><br>will give output of <br><br>Array<br>(<br>    [test1] =&gt; asgafasdaad<br>    [TEST2] =&gt; asddhshsdgb<br>    [TeSt3] =&gt; asdasda@asdadadasdasdgh<br>)<br>Array<br>(<br>    [test1] =&gt; asgafasdaad<br>    [test2] =&gt; asddhshsdgb<br>    [test3] =&gt; asdasda@asdadadasdasdgh<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.strtolower.php)

**[To root](/README.md)**