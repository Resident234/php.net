# strtolower



strtolower(); doesn&apos;t work for polish chars<br><br>

```
<?php strtolower("m&#x104;kA"); ?>
```

will return: m&#x104;ka;

the best solution - use mb_strtolower()



```
<?php mb_strtolower("m&#x104;kA",&apos;UTF-8&apos;); ?>
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
$array = array(&apos;test1&apos; =&gt; &apos;asgAFasDAAd&apos;, &apos;TEST2&apos; =&gt; &apos;ASddhshsDGb&apos;, &apos;TeSt3 &apos;=&gt; &apos;asdasda@asdadadASDASDgh&apos;);

$array = arraytolower($array);
?>
```

/*
Array
(
    [test1] =&gt; asgafasdaad
    [TEST2] =&gt; ASddhshsDGb
    [TeSt3] =&gt; asdasda@asdadadASDASDgh
    [test2] =&gt; asddhshsdgb
    [test3] =&gt; asdasda@asdadadasdasdgh
)
*/

I prefer this method



```
<?php
  function arraytolower($array, $include_leys=false) {
    
    if($include_leys) {
      foreach($array as $key =&gt; $value) {
        if(is_array($value))
          $array2[strtolower($key)] = arraytolower($value, $include_leys);
        else
          $array2[strtolower($key)] = strtolower($value);
      }
      $array = $array2;
    }
    else {
      foreach($array as $key =&gt; $value) {
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
$array = $array = array(&apos;test1&apos; =&gt; &apos;asgAFasDAAd&apos;, &apos;TEST2&apos; =&gt; &apos;ASddhshsDGb&apos;, &apos;TeSt3 &apos;=&gt; &apos;asdasda@asdadadASDASDgh&apos;);

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