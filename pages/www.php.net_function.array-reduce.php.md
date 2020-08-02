# array_reduce



To make it clearer about what the two parameters of the callback are for, and what "reduce to a single value" actually means (using associative and commutative operators as examples may obscure this).<br><br>The first parameter to the callback is an accumulator where the result-in-progress is effectively assembled. If you supply an $initial value the accumulator starts out with that value, otherwise it starts out null.<br>The second parameter is where each value of the array is passed during each step of the reduction.<br>The return value of the callback becomes the new value of the accumulator. When the array is exhausted, array_reduce() returns accumulated value.<br><br>If you carried out the reduction by hand, you&apos;d get something like the following lines, every one of which therefore producing the same result:<br>

```
<?php
array_reduce(array(1,2,3,4), 'f',         99             );
array_reduce(array(2,3,4),   'f',       f(99,1)          );
array_reduce(array(3,4),     'f',     f(f(99,1),2)       );
array_reduce(array(4),       'f',   f(f(f(99,1),2),3)    );
array_reduce(array(),        'f', f(f(f(f(99,1),2),3),4) );
f(f(f(f(99,1),2),3),4)
?>
```


If you made function f($v,$w){return "f($v,$w)";} the last line would be the literal result.

A PHP implementation might therefore look something like this (less details like error checking and so on):


```
<?php
function array_reduce($array, $callback, $initial=null)
{
    $acc = $initial;
    foreach($array as $a)
        $acc = $callback($acc, $a);
    return $acc;
}
?>
```
  

---

Sometimes we need to go through an array and group the indexes so that it is easier and easier to extract them in the iteration.<br><br>

```
<?php

$people = [
    ['id' => 1, 'name' => 'Hayley'],
    ['id' => 2, 'name' => 'Jack', 'dad' => 1],
    ['id' => 3, 'name' => 'Linus', 'dad'=> 4],
    ['id' => 4, 'name' => 'Peter' ],
    ['id' => 5, 'name' => 'Tom', 'dad' => 4],
];

$family = array_reduce($people, function($accumulator, $item) {
    // if you don't have a dad you are probably a dad
    if (!isset($item['dad'])) {
        $id = $item['id'];
        $name = $item['name'];
        // take the children if you already have 
        $children = $accumulator[$id]['children'] ?? [];
        // add dad
        $accumulator[$id] = ['id' => $id, 'name' => $name,'children' => $children];
        return $accumulator;
    }

    // add a new dad if you haven't already 
    $dad = $item['dad'];
    if (!isset($accumulator[$dad])) {
        // how did you find the dad will first add only with children 
        $accumulator[$dad] = ['children' => [$item]];
        return $accumulator;
    }

    //  add a son to his dad who has already been added
    //  by the first or second conditional "if"
    
    $accumulator[$dad]['children'][] = $item;
    return $accumulator;
}, []);

var_export(array_values($family));

?>
```


OUTPUT

array (
  0 =>
  array (
    'id' => 1,
    'name' => 'Hayley',
    'children' =>
    array (
      0 =>
      array (
        'id' => 2,
        'name' => 'Jack',
        'dad' => 1,
      ),
    ),
  ),
  1 =>
  array (
    'id' => 4,
    'name' => 'Peter',
    'children' =>
    array (
      0 =>
      array (
        'id' => 3,
        'name' => 'Linus',
        'dad' => 4,
      ),
      1 =>
      array (
        'id' => 5,
        'name' => 'Tom',
        'dad' => 4,
      ),
    ),
  ),
)



```
<?php
$array = [
  [
    "menu_id" => "1",
    "menu_name" => "Clients",
    "submenu_name" => "Add",
    "submenu_link" => "clients/add"
  ],
  [
    "menu_id" => "1",
    "menu_name" => "Clients",
    "submenu_name" => "List",
    "submenu_link" => "clients"
  ],
  [
    "menu_id" => "2",
    "menu_name" => "Products",
    "submenu_name" => "List",
    "submenu_link" => "products"
  ],
];

//Grouping submenus to their menus

$menu =  array_reduce($array, function($accumulator, $item){
  $index = $item['menu_name'];

  if (!isset($accumulator[$index])) {
    $accumulator[$index] = [
      'menu_id' => $item['menu_id'],
      'menu_name' => $item['menu_name'],
      'submenu' => []    
    ];
  }

  $accumulator[$index]['submenu'][] = [
    'submenu_name' => $item['submenu_name'],
    'submenu_link' => $item['submenu_link']
  ];

  return $accumulator;
}, []);

var_export(array_values($menu));

?>
```
<br><br>OUTPUT<br><br>array (<br>  0 =&gt;<br>  array (<br>    &apos;menu_id&apos; =&gt; &apos;1&apos;,<br>    &apos;menu_name&apos; =&gt; &apos;Clients&apos;,<br>    &apos;submenu&apos; =&gt;<br>    array (<br>      0 =&gt;<br>      array (<br>        &apos;submenu_name&apos; =&gt; &apos;Add&apos;,<br>        &apos;submenu_link&apos; =&gt; &apos;clients/add&apos;,<br>      ),<br>      1 =&gt;<br>      array (<br>        &apos;submenu_name&apos; =&gt; &apos;List&apos;,<br>        &apos;submenu_link&apos; =&gt; &apos;clients&apos;,<br>      ),<br>    ),<br>  ),<br>  1 =&gt;<br>  array (<br>    &apos;menu_id&apos; =&gt; &apos;2&apos;,<br>    &apos;menu_name&apos; =&gt; &apos;Products&apos;,<br>    &apos;submenu&apos; =&gt;<br>    array (<br>      0 =&gt;<br>      array (<br>        &apos;submenu_name&apos; =&gt; &apos;List&apos;,<br>        &apos;submenu_link&apos; =&gt; &apos;products&apos;,<br>      ),<br>    ),<br>  ),<br>)  

---

So, if you were wondering how to use this where key and value are passed in to the function. I&apos;ve had success with the following (this example generates formatted html attributes from an associative array of attribute =&gt; value pairs):<br><br>

```
<?php

    // Attribute List
    $attribs = [
        'name' => 'first_name',
        'value' => 'Edward'
    ];

    // Attribute string formatted for use inside HTML element
    $formatted_attribs = array_reduce(
        array_keys($attribs),                       // We pass in the array_keys instead of the array here
        function ($carry, $key) use ($attribs) {    // ... then we 'use' the actual array here
            return $carry . ' ' . $key . '="' . htmlspecialchars( $attribs[$key] ) . '"';
        },
        ''
    );

echo $formatted_attribs;

?>
```
<br><br>This will output:<br> name="first_name" value="Edward"  

---

You can effectively ignore the fact $result is passed into the callback by reference. Only the return value of the callback is accounted for.<br><br>

```
<?php

$arr = [1,2,3,4];

var_dump(array_reduce(
    $arr,
    function(&amp;$res, $a) { $res += $a; }, 
    0
));

# NULL

?>
```




```
<?php

$arr = [1,2,3,4];

var_dump(array_reduce(
    $arr, 
    function($res, $a) { return $res + $a;  }, 
    0
));

# int(10)
?>
```
<br><br>Be warned, though, that you *can* accidentally change $res if it&apos;s not a simple scalar value, so despite the examples I&apos;d recommend not writing to it at all.  

---

If you do not provide $initial, the first value used in the iteration is NULL. This is not a problem for callback functions that treat NULL as an identity (e.g. addition), but is a problem for cases when NULL is not identity (such as boolean context).<br><br>Compare:<br><br>

```
<?php
function andFunc($a, $b) {
  return $a &amp;&amp; $b;
}
$foo = array(true, true, true);
var_dump(array_reduce($foo, "andFunc"));
?>
```
<br><br>returns false! One would expect that it would return true because `true &amp;&amp; true &amp;&amp; true == true`!<br><br>Adding diagnostic output to andFunc() shows that the first call to andFunc is with the arguments (NULL, true). This resolves to false (as `(bool) null == false`) and thereby corrupts the whole reduction.<br><br>So in this case I have to set `$initial = true` so that the first call to andFunc() will be (true, true). Now, if I were doing, say, orFunc(), I would have to set `$initial = false`. Beware.<br><br>Note that the "rmul" case in the example sneakily hides this defect! They use an $initial of 10 to get `10*1*2*3*4*5 = 12000`. So you would assume that without an initial, you would get `1200/10 = 120 = 1*2*3*4*5`. Nope! You get big fat zero, because `int(null)==0`, and `0*1*2*3*4*5 = 0`!<br><br>I don&apos;t honestly see why array_reduce starts with a null argument. The first call to the callback should be with arguments ($initial[0],$initial[1]) [or whatever the first two array entries are], not (null,$initial[0]). That&apos;s what one would expect from the description.<br><br>Incidentally this also means that under the current implementation you will incur `count($input)` number of calls to the callback, not `count($input) - 1` as you might expect.  

---

[Official documentation page](https://www.php.net/manual/en/function.array-reduce.php)

**[To root](/README.md)**