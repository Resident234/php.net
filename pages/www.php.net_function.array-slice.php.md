# array_slice



Array slice function that works with associative arrays (keys):<br><br>function array_slice_assoc($array,$keys) {<br>    return array_intersect_key($array,array_flip($keys));<br>}  

---

array_slice can be used to remove elements from an array but it&apos;s pretty simple to use a custom function.<br><br>One day array_remove() might become part of PHP and will likely be a reserved function name, hence the unobvious choice for this function&apos;s names.<br><br>

```
<?php
function arem($array,$value){
    $holding=array();
    foreach($array as $k => $v){
        if($value!=$v){
            $holding[$k]=$v;
        }
    }    
    return $holding;
}

function akrem($array,$key){
    $holding=array();
    foreach($array as $k => $v){
        if($key!=$k){
            $holding[$k]=$v;
        }
    }    
    return $holding;
}

$lunch = array('sandwich' => 'cheese', 'cookie'=>'oatmeal','drink' => 'tea','fruit' => 'apple');
echo '';
print_r($lunch);
$lunch=arem($lunch,'apple');
print_r($lunch);
$lunch=akrem($lunch,'sandwich');
print_r($lunch);
echo '';
?>
```
<br><br>(remove 9&apos;s in email)  

---



```
<?php
// CHOP $num ELEMENTS OFF THE FRONT OF AN ARRAY
// RETURN THE CHOP, SHORTENING THE SUBJECT ARRAY
function array_chop(&amp;$arr, $num)
{
    $ret = array_slice($arr, 0, $num);
    $arr = array_slice($arr, $num);
    return $ret;
}?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.array-slice.php)

**[To root](/README.md)**