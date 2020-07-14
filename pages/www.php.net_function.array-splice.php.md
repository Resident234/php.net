# array_splice



array_splice, split an array into 2 arrays. The returned arrays is the 2nd argument actually and the used array e.g $input here contains the 1st argument of array, e.g<br><br>

```
<?php
$input = array("red", "green", "blue", "yellow");
print_r(array_splice($input, 3)); // Array ( [0] =&gt; yellow )  
print_r($input); //Array ( [0] =&gt; red [1] =&gt; green [2] =&gt; blue )
?>
```


if you want to replace any array value do simple like that,

first search the array index you want to replace



```
<?php $index = array_search(&apos;green&apos;, $input);// index = 1 ?>
```


and then use it as according to the definition



```
<?php
array_splice($input, $index, 1, array(&apos;mygreeen&apos;)); //Array ( [0] =&gt; red [1] =&gt; mygreeen [2] =&gt; blue [3] =&gt; yellow ) 
?>
```
<br><br>so here green is replaced by mygreen.<br><br>here 1 in array_splice above represent the number of items to be replaced. so here start at index &apos;1&apos; and replaced only one item which is &apos;green&apos;  

#

You cannot insert with array_splice an array with your own key. array_splice will always insert it with the key "0".<br><br>

```
<?php
// [DATA]
$test_array = array (
  row1 =&gt; array (col1 =&gt; &apos;foobar!&apos;, col2 =&gt; &apos;foobar!&apos;),
  row2 =&gt; array (col1 =&gt; &apos;foobar!&apos;, col2 =&gt; &apos;foobar!&apos;),
  row3 =&gt; array (col1 =&gt; &apos;foobar!&apos;, col2 =&gt; &apos;foobar!&apos;)
);

// [ACTION]
array_splice ($test_array, 2, 0, array (&apos;rowX&apos; =&gt; array (&apos;colX&apos; =&gt; &apos;foobar2&apos;)));
echo &apos;&lt;pre&gt;&apos;; print_r ($test_array); echo &apos;&lt;/pre&gt;&apos;;
?>
```


[RESULT]

Array (
    [row1] =&gt; Array (
            [col1] =&gt; foobar!
            [col2] =&gt; foobar!
        )

    [row2] =&gt; Array (
            [col1] =&gt; foobar!
            [col2] =&gt; foobar!
        )

    [0] =&gt; Array (
            [colX] =&gt; foobar2
        )

    [row3] =&gt; Array (
            [col1] =&gt; foobar!
            [col2] =&gt; foobar!
        )
)

But you can use the following function:

function array_insert (&amp;$array, $position, $insert_array) {
  $first_array = array_splice ($array, 0, $position);
  $array = array_merge ($first_array, $insert_array, $array);
}



```
<?php
// [ACTION]

array_insert ($test_array, 2, array (&apos;rowX&apos; =&gt; array (&apos;colX&apos; =&gt; &apos;foobar2&apos;)));
echo &apos;&lt;pre&gt;&apos;; print_r ($test_array); echo &apos;&lt;/pre&gt;&apos;;
?>
```
<br><br>[RESULT]<br><br>Array (<br>    [row1] =&gt; Array (<br>            [col1] =&gt; foobar!<br>            [col2] =&gt; foobar!<br>        )<br><br>    [row2] =&gt; Array (<br>            [col1] =&gt; foobar!<br>            [col2] =&gt; foobar!<br>        )<br><br>    [rowX] =&gt; Array (<br>            [colX] =&gt; foobar2<br>        )<br><br>    [row3] =&gt; Array (<br>            [col1] =&gt; foobar!<br>            [col2] =&gt; foobar!<br>        )<br>)<br><br>[NOTE]<br><br>The position "0" will insert the array in the first position (like array_shift). If you try a position higher than the langth of the array, you add it to the array like the function array_push.  

#

just useful functions to move an element using array_splice.<br><br>

```
<?php

// info at danielecentamore dot com

// $input  (Array) - the array containing the element
// $index (int) - the index of the element you need to move

function moveUp($input,$index) {
      $new_array = $input;
      
       if((count($new_array)&gt;$index) &amp;&amp; ($index&gt;0)){
                 array_splice($new_array, $index-1, 0, $input[$index]);
                 array_splice($new_array, $index+1, 1);
             } 

       return $new_array;
}

function moveDown($input,$index) {
       $new_array = $input;
         
       if(count($new_array)&gt;$index) {
                 array_splice($new_array, $index+2, 0, $input[$index]);
                 array_splice($new_array, $index, 1);
             } 
   
       return $new_array;
 }  

$input = array("red", "green", "blue", "yellow");

$newinput = moveUp($input, 2);
// $newinput is array("red", "blue", "green", "yellow")

$input = moveDown($newinput, 1);
// $input is array("red", "green", "blue", "yellow")

?>
```
  

#

When trying to splice an associative array into another, array_splice is missing two key ingredients:<br>  - a string key for identifying the offset<br>  - the ability to preserve keys in the replacement array<br><br>This is primarily useful when you want to replace an item in an array with another item, but want to maintain the ordering of the array without rebuilding the array one entry at a time.<br><br>

```
<?php
function array_splice_assoc(&amp;$input, $offset, $length, $replacement) {
        $replacement = (array) $replacement;
        $key_indices = array_flip(array_keys($input));
        if (isset($input[$offset]) &amp;&amp; is_string($offset)) {
                $offset = $key_indices[$offset];
        }
        if (isset($input[$length]) &amp;&amp; is_string($length)) {
                $length = $key_indices[$length] - $offset;
        }

        $input = array_slice($input, 0, $offset, TRUE)
                + $replacement
                + array_slice($input, $offset + $length, NULL, TRUE);
}

$fruit = array(
        &apos;orange&apos; =&gt; &apos;orange&apos;,
        &apos;lemon&apos; =&gt; &apos;yellow&apos;,
        &apos;lime&apos; =&gt; &apos;green&apos;,
        &apos;grape&apos; =&gt; &apos;purple&apos;,
        &apos;cherry&apos; =&gt; &apos;red&apos;,
);

// Replace lemon and lime with apple
array_splice_assoc($fruit, &apos;lemon&apos;, &apos;grape&apos;, array(&apos;apple&apos; =&gt; &apos;red&apos;));

// Replace cherry with strawberry
array_splice_assoc($fruit, &apos;cherry&apos;, 1, array(&apos;strawberry&apos; =&gt; &apos;red&apos;));
?>
```
<br><br>Note: I have not tested this with negative offsets and lengths.  

#

[Official documentation page](https://www.php.net/manual/en/function.array-splice.php)

**[To root](/README.md)**