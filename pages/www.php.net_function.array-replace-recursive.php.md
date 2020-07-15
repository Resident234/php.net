# array_replace_recursive



Nice that this function finally found its was to the PHP core! If you want to use it also with older PHP versions before 5.3.0, you can define it this way:<br><br>

```
<?php
if (!function_exists('array_replace_recursive'))
{
  function array_replace_recursive($array, $array1)
  {
    function recurse($array, $array1)
    {
      foreach ($array1 as $key => $value)
      {
        // create new key in $array, if it is empty or not an array
        if (!isset($array[$key]) || (isset($array[$key]) &amp;&amp; !is_array($array[$key])))
        {
          $array[$key] = array();
        }
  
        // overwrite the value in the base array
        if (is_array($value))
        {
          $value = recurse($array[$key], $value);
        }
        $array[$key] = $value;
      }
      return $array;
    }
  
    // handle the arguments, merge one by one
    $args = func_get_args();
    $array = $args[0];
    if (!is_array($array))
    {
      return $array;
    }
    for ($i = 1; $i &lt; count($args); $i++)
    {
      if (is_array($args[$i]))
      {
        $array = recurse($array, $args[$i]);
      }
    }
    return $array;
  }
}
?>
```


I called this function array_merge_recursive_overwrite() in my older projects, but array_replace_recursive() sounds quite better while they do the same.

If you implemented such a compatible function before and don't want to refactor all your code, you can update it with the following snippet to use the native (and hopefully faster) implementation of PHP 5.3.0, if available. Just start your function with these lines:



```
<?php
  // as of PHP 5.3.0 array_replace_recursive() does the work for us
  if (function_exists('array_replace_recursive'))
  {
    return call_user_func_array('array_replace_recursive', func_get_args());
  }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-replace-recursive.php)

**[To root](/README.md)**