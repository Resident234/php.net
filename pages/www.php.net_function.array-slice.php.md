# array_slice



Array slice function that works with associative arrays (keys):<br><br>function array_slice_assoc($array,$keys) {<br>    return array_intersect_key($array,array_flip($keys));<br>}  

#

array_slice can be used to remove elements from an array but it&apos;s pretty simple to use a custom function.<br><br>One day array_remove() might become part of PHP and will likely be a reserved function name, hence the unobvious choice for this function&apos;s names.<br><br>&lt;?<br>function arem($array,$value){<br>    $holding=array();<br>    foreach($array as $k =&gt; $v){<br>        if($value!=$v){<br>            $holding[$k]=$v;<br>        }<br>    }    <br>    return $holding;<br>}<br><br>function akrem($array,$key){<br>    $holding=array();<br>    foreach($array as $k =&gt; $v){<br>        if($key!=$k){<br>            $holding[$k]=$v;<br>        }<br>    }    <br>    return $holding;<br>}<br><br>$lunch = array(&apos;sandwich&apos; =&gt; &apos;cheese&apos;, &apos;cookie&apos;=&gt;&apos;oatmeal&apos;,&apos;drink&apos; =&gt; &apos;tea&apos;,&apos;fruit&apos; =&gt; &apos;apple&apos;);<br>echo &apos;&lt;pre&gt;&apos;;<br>print_r($lunch);<br>$lunch=arem($lunch,&apos;apple&apos;);<br>print_r($lunch);<br>$lunch=akrem($lunch,&apos;sandwich&apos;);<br>print_r($lunch);<br>echo &apos;&lt;/pre&gt;&apos;;<br>?>
```
<br><br>(remove 9&apos;s in email)  

#



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
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-slice.php)

**[To root](/README.md)**