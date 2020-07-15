# array_filter



If you want a quick way to remove NULL, FALSE and Empty Strings (""), but leave values of 0 (zero), you can use the standard php function strlen as the callback function:<br>eg:<br>

```
<?php

// removes all NULL, FALSE and Empty Strings but leaves 0 (zero) values
$result = array_filter( $array, 'strlen' );

?>
```
  

#

Because array_filter() preserves keys, you should consider the resulting array to be an associative array even if the original array had integer keys for there may be holes in your sequence of keys. This means that, for example, json_encode() will convert your result array into an object instead of an array. Call array_values() on the result array to guarantee json_encode() gives you an array.  

#

In case you are interested (like me) in filtering out elements with certain key-names, array_filter won&apos;t help you. Instead you can use the following:<br><br>

```
<?php
$arr = array( 'element1' => 1, 'element2' => 2, 'element3' => 3, 'element4' => 4 );
$filterOutKeys = array( 'element1', 'element4' );

$filteredArr = array_diff_key( $arr, array_flip( $filterOutKeys ) )
?>
```
<br><br>Result will be something like this:<br>[&apos;element2&apos;] =&gt; 2<br>[&apos;element3&apos;] =&gt; 3  

#

Here is how you could easily delete a specific value from an array with array_filter:<br><br>

```
<?php
$array = array (1, 3, 3, 5, 6);
$my_value = 3;
$filtered_array = array_filter($array, function ($element) use ($my_value) { return ($element != $my_value); } );
print_r($filtered_array);
?>
```
<br><br>output:<br><br>Array<br>(<br>    [0] =&gt; 1<br>    [3] =&gt; 5<br>    [4] =&gt; 6<br>)  

#

array_filter remove also FALSE and 0. To remove only NULL&apos;s use:<br><br>$af = [1, 0, 2, null, 3, 6, 7];<br><br>function is_not_null($val){<br>    return !is_null($val);<br>}<br>$af = array_filter($af, &apos;is_not_null&apos;);  

#

Here&apos;s a function that will filter a multi-demensional array. This filter will return only those items that match the $value given<br><br>

```
<?php
    /*
     * filtering an array
     */
    function filter_by_value ($array, $index, $value){ 
        if(is_array($array) &amp;&amp; count($array)&gt;0)  
        { 
            foreach(array_keys($array) as $key){
                $temp[$key] = $array[$key][$index];
                 
                if ($temp[$key] == $value){
                    $newarray[$key] = $array[$key];
                }
            }
          }
      return $newarray; 
    } 
?>
```


Example:



```
<?php
$results = array(
   0 => array('key1' => '1', 'key2' => 2, 'key3' => 3),
   1 => array('key1' => '12', 'key2' => 22, 'key3' => 32) 
);

$nResults = filter_by_value($results, 'key2', '2');
?>
```
<br><br>Output :<br><br>array(<br>    0 =&gt; array(&apos;key1&apos; =&gt; &apos;1&apos;, &apos;key2&apos; =&gt; 2, &apos;key3&apos; =&gt; 3)<br>);  

#

If you want to use array_filter with a class method as the callback, you can use a psuedo type callback like this:<br><br>

```
<?php
class Test
{
    public function doFilter($array)
    {
        return array_filter($array, array($this, 'callbackMethodName'));
    }

    protected function callbackMethodName($element)
    {
        return $element % 2 === 0;
    }
}

$example = new Test;
print_r($example->doFilter(range(1, 10)));
?>
```
<br><br>Will return even numbers.  

#

Some of PHP&apos;s array functions play a prominent role in so called functional programming languages, where they show up under a slightly different name:<br><br>

```
<?php
  array_filter() -> filter(), 
  array_map() -> map(), 
  array_reduce() -> foldl() ("fold left")
?>
```
<br><br>Functional programming is a paradigm which centers around the side-effect free evaluation of functions. A program execution is a call of a function, which in turn might be defined by many other functions. One idea is to use functions to create special purpose functions from other functions.<br><br>The array functions mentioned above allow you compose new functions on arrays. <br><br>E.g. array_sum = array_map("sum", $arr).<br><br>This leads to a style of programming that looks much like algebra, e.g. the Bird/Meertens formalism.<br><br>E.g. a mathematician might state<br><br>  map(f o g) = map(f) o map(g)<br><br>the so called "loop fusion" law.<br><br>Many functions on arrays can be created by the use of the foldr() function (which works like foldl, but eating up array elements from the right).<br><br>I can&apos;t get into detail here, I just wanted to provide a hint about where this stuff also shows up and the theory behind it.  

#

This function filters an array and remove all null values recursively.<br><br>

```
<?php
  function array_filter_recursive($input)
  {
    foreach ($input as &amp;$value)
    {
      if (is_array($value))
      {
        $value = array_filter_recursive($value);
      }
    }
    
    return array_filter($input);
  }
?>
```


Or with callback parameter (not tested) :



```
<?php
  function array_filter_recursive($input, $callback = null)
  {
    foreach ($input as &amp;$value)
    {
      if (is_array($value))
      {
        $value = array_filter_recursive($value, $callback);
      }
    }
    
    return array_filter($input, $callback);
  }
?>
```
  

#

You can access the current key of array by passing a reference to array into callback function and call key() and next() method in the callback function:<br>

```
<?php
$data = array('first' => 1, 'second' => 2, 'third' => 3);
$data = array_filter($data, function ($item) use (&amp;$data) {
    echo "Filtering key ", key($data), '&lt;br&gt;', PHP_EOL;
    next($data);
    return false;
});
?>
```
<br><br>However be careful with array internal pointer or use reset() method before calling array_filter().  

#

[Official documentation page](https://www.php.net/manual/en/function.array-filter.php)

**[To root](/README.md)**