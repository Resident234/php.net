# count



[Editor&apos;s note: array at from dot pl had pointed out that count() is a cheap operation; however, there&apos;s still the function call overhead.]<br><br>If you want to run through large arrays don&apos;t use count() function in the loops , its a over head in performance,  copy the count() value into a variable and use that value in loops for a better performance.<br><br>Eg:<br><br>// Bad approach<br><br>for($i=0;$i&lt;count($some_arr);$i++)<br>{<br>    // calculations<br>}<br><br>// Good approach<br><br>$arr_length = count($some_arr);<br>for($i=0;$i&lt;$arr_length;$i++)<br>{<br>    // calculations<br>}  

---

My function returns the number of elements in array for multidimensional arrays subject to depth of array. (Almost COUNT_RECURSIVE, but you can point on which depth you want to plunge).<br><br>

```
<?php
  function getArrCount ($arr, $depth=1) {
      if (!is_array($arr) || !$depth) return 0;
         
     $res=count($arr);
         
      foreach ($arr as $in_ar)
         $res+=getArrCount($in_ar, $depth-1);
      
      return $res;
  }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.count.php)

**[To root](/README.md)**