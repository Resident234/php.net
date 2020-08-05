# array_merge



In some situations, the union operator ( + ) might be more useful to you than array_merge.  The array_merge function does not preserve numeric key values.  If you need to preserve the numeric keys, then using + will do that.<br><br>ie:<br><br>

```
<?php

$array1[0] = "zero";
$array1[1] = "one";

$array2[1] = "one";
$array2[2] = "two";
$array2[3] = "three";

$array3 = $array1 + $array2;

//This will result in::

$array3 = array(0=>"zero", 1=>"one", 2=>"two", 3=>"three");

?>
```
<br><br>Note the implicit "array_unique" that gets applied as well.  In some situations where your numeric keys matter, this behaviour could be useful, and better than array_merge.<br><br>--Julian  

---

As PHP 5.6 you can use array_merge + "splat" operator to reduce a bidimensonal array to a simple array:<br><br>

```
<?php
$data = [[1, 2], [3], [4, 5]];
    print_r(array_merge(... $data)); // [1, 2, 3, 4, 5];
?>
```


PHP < 5.6:



```
<?php
$data = [[1, 2], [3], [4, 5]];
print_r(array_reduce($data, 'array_merge', []));   // [1, 2, 3, 4, 5];

?>
```
  

---

Sometimes we need to traverse an array and group / merge the indexes so that it is easier to extract them so that they are related in the iteration.<br><br>

```
<?php

$people = [
    ['id' => 1, 'name' => 'Hayley'],
    ['id' => 2, 'name' => 'Jack', 'dad' => 1],
    ['id' => 3, 'name' => 'Linus', 'dad' => 4],
    ['id' => 4, 'name' => 'Peter'],
    ['id' => 5, 'name' => 'Tom', 'dad' => 4],
];

// We set up an array with just the children
function children($dad, $people)
{
    $children = [];
    foreach ($people as $p) {
        if (!empty($p["dad"]) &amp;&amp; $p["dad"] == $dad["id"]) {
            $children[] = $p;
        }
    }

    return $children;
}

$family = [];

// We merge each child with its respective parent
foreach ($people as $p) {
    $children = children($p, $people);
    if ($children != []) {
        $family[] = array_merge($p, ["children" => $children]);
    }
}

print_r($family);

?>
```
<br><br>//OUTPUT<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [id] =&gt; 1<br>            [name] =&gt; Hayley<br>            [children] =&gt; Array<br>                (<br>                    [0] =&gt; Array<br>                        (<br>                            [id] =&gt; 2<br>                            [name] =&gt; Jack<br>                            [dad] =&gt; 1<br>                        )<br><br>                )<br><br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [id] =&gt; 4<br>            [name] =&gt; Peter<br>            [children] =&gt; Array<br>                (<br>                    [0] =&gt; Array<br>                        (<br>                            [id] =&gt; 3<br>                            [name] =&gt; Linus<br>                            [dad] =&gt; 4<br>                        )<br><br>                    [1] =&gt; Array<br>                        (<br>                            [id] =&gt; 5<br>                            [name] =&gt; Tom<br>                            [dad] =&gt; 4<br>                        )<br><br>                )<br><br>        )<br><br>)  

---

An addition to what Julian Egelstaff above wrote - the array union operation (+) is not doing an array_unique - it will just not use the keys that are already defined in the left array.  The difference between union and merge can be seen in an example like this:<br><br>

```
<?php
$arr1['one'] = 'one';
$arr1['two'] = 'two';

$arr2['zero'] = 'zero';
$arr2['one'] = 'three';
$arr2['two'] = 'four';

$arr3 = $arr1 + $arr2;
var_export( $arr3 );
# array ( 'one' => 'one', 'two' => 'two', 'zero' => 'zero', )

$arr4 = array_merge( $arr1, $arr2 );
var_export( $arr4 );
# array ( 'one' => 'three', 'two' => 'four', 'zero' => 'zero', )
?>
```
  

---

to get unique value from multi dimensional array use this instead of array_unique(), because array_unique() does not work on multidimensional:<br>array_map("unserialize", array_unique(array_map("serialize", $array)));<br>Hope this will help someone;<br>Example<br>$a=array(array(&apos;1&apos;),array(&apos;2&apos;),array(&apos;3&apos;),array(&apos;4));<br>$b=array(array(&apos;2&apos;),array(&apos;4&apos;),array(&apos;6&apos;),array(&apos;8));<br>$c=array_merge($a,$b);<br>then write this line to get unique values<br>$c=array_map("unserialize", array_unique(array_map("serialize", $c)));<br>print_r($c);  

---

I constantly forget the direction of array_merge so this is partially for me and partially for people like me.<br><br>array_merge is a non-referential non-inplace right-reduction. For different ctrl-f typers, it&apos;s reduce-right, side-effect free, idempotent, and non in-place.<br><br>ex:<br><br>$a = array_merge([&apos;k&apos; =&gt; &apos;a&apos;], [&apos;k&apos; =&gt; &apos;b&apos;]) =&gt; [&apos;k&apos; =&gt; &apos;b&apos;]<br>array_merge([&apos;z&apos; =&gt; 1], $a) =&gt; does not modify $a but returns [&apos;k&apos; =&gt; &apos;b&apos;, &apos;z&apos; =&gt; 1];<br><br>Hopefully this helps people that constant look this up such as myself.  

---

Reiterating the notes about casting to arrays, be sure to cast if one of the arrays might be null:<br><br>

```
<?php
header("Content-type:text/plain");
$a = array('zzzz', 'xxxx');
$b = array('mmmm','nnnn');

echo "1 ==============\r\n";
print_r(array_merge($a, $b));

echo "2 ==============\r\n";
$b = array();
print_r(array_merge($a, $b));

echo "3 ==============\r\n";
$b = null;
print_r(array_merge($a, $b));

echo "4 ==============\r\n";
$b = null;
print_r(array_merge($a, (array)$b));

echo "5 ==============\r\n";
echo is_null(array_merge($a, $b)) ? 'Result is null' : 'Result is not null';
?>
```
<br><br>Produces:<br><br>1 ==============<br>Array<br>(<br>    [0] =&gt; zzzz<br>    [1] =&gt; xxxx<br>    [2] =&gt; mmmm<br>    [3] =&gt; nnnn<br>)<br>2 ==============<br>Array<br>(<br>    [0] =&gt; zzzz<br>    [1] =&gt; xxxx<br>)<br>3 ==============<br>4 ==============<br>Array<br>(<br>    [0] =&gt; zzzz<br>    [1] =&gt; xxxx<br>)<br>5 ==============<br>Result is null  

---

if you generate form select from an array, you probably want to keep your array keys and order intact,<br>if so you can use ArrayMergeKeepKeys(), works just like array_merge :<br><br>array ArrayMergeKeepKeys ( array array1 [, array array2 [, array ...]])<br><br>but keeps the keys even if of numeric kind. <br>enjoy<br><br>

```
<?php

$Default[0]='Select Something please';

$Data[147]='potato';
$Data[258]='banana';
$Data[54]='tomato';

$A=array_merge($Default,$Data);

$B=ArrayMergeKeepKeys($Default,$Data);

echo '';
print_r($A);
print_r($B);
echo '';

Function ArrayMergeKeepKeys() {
      $arg_list = func_get_args();
      foreach((array)$arg_list as $arg){
          foreach((array)$arg as $K => $V){
              $Zoo[$K]=$V;
          }
      }
    return $Zoo;
}

//will output :

Array
(
    [0] => Select Something please
    [1] => potato
    [2] => banana
    [3] => tomato
)
Array
(
    [0] => Select Something please
    [147] => potato
    [258] => banana
    [54] => tomato
)

?>
```
  

---

I keep seeing posts for people looking for a function to replace numeric keys.<br><br>No function is required for this, it is default behavior if the + operator:<br><br>

```
<?php
$a=array(1=>"one", "two"=>2);
$b=array(1=>"two", "two"=>1, 3=>"three", "four"=>4);

print_r($a+$b);
?>
```
<br><br>Array<br>(<br>    [1] =&gt; one<br>    [two] =&gt; 2<br>    [3] =&gt; three<br>    [four] =&gt; 4<br>)<br><br>How this works:<br><br>The + operator only adds unique keys to the resulting array.  By making the replacements the first argument, they naturally always replace the keys from the second argument, numeric or not! =)  

---

[Official documentation page](https://www.php.net/manual/en/function.array-merge.php)

**[To root](/README.md)**