# The RecursiveIteratorIterator class



Some speed tests<br>

```
<?php
$timer = function ($name = &apos;default&apos;, $unset_timer = TRUE)
{
    static $timers = array();
    
    if ( isset( $timers[ $name ] ) )
    {
        list($s_sec, $s_mic) = explode(&apos; &apos;, $timers[ $name ]);
        list($e_sec, $e_mic) = explode(&apos; &apos;, microtime());
        
        if ( $unset_timer )
            unset( $timers[ $name ] );
        
        return $e_sec - $s_sec + ( $e_mic - $s_mic );
    }
    
    $timers[ $name ] = microtime();
};

function f1 ($array) {
    $iterator = new RecursiveIteratorIterator(new RecursiveArrayIterator($array), RecursiveIteratorIterator::SELF_FIRST);

    foreach ( $iterator as $key =&gt; $value ) {
        if ( is_array($value) )
            continue;
    }
}

function f2($array) {
    foreach ( $array as $key =&gt; $value ) {
        if ( is_array($value) )
            f2($value);
    }
}

foreach ( [100, 1000, 10000, 100000, 1000000] as $num )
{
    $array = [];
    
    for ( $i = 0; ++$i &lt; $num; )
        $array[] = [1,2,3=&gt;[4,5,6=&gt;[7,8,9=&gt;10,11,12=&gt;[13,14,15=&gt;[16,17,18]]]]];
    
    $timer();
    f1($array);
    printf("RecursiveIteratorIterator: %7d elements -&gt; %.3f sec\n", $num, $timer());
    
    $timer();
    f2($array);
    printf("Recursive function       : %7d elements -&gt; %.3f sec\n", $num, $timer());
}

?>
```
<br><br>Output (PHP 5.4.9-4ubuntu2.1 (cli) (built: Jun 11 2013 13:10:01))<br>=======================<br>RecursiveIteratorIterator:     100 elements -&gt; 0.007 sec<br>Recursive function       :     100 elements -&gt; 0.002 sec<br>RecursiveIteratorIterator:    1000 elements -&gt; 0.036 sec<br>Recursive function       :    1000 elements -&gt; 0.024 sec<br>RecursiveIteratorIterator:   10000 elements -&gt; 0.425 sec<br>Recursive function       :   10000 elements -&gt; 0.263 sec<br>RecursiveIteratorIterator:  100000 elements -&gt; 8.153 sec<br>Recursive function       :  100000 elements -&gt; 2.654 sec<br>RecursiveIteratorIterator: 1000000 elements -&gt; 474.483 sec<br>Recursive function       : 1000000 elements -&gt; 26.872 sec<br><br>For one million elements recursive function is more quickly!  

#

A very useful use case for RecusiveIteratorIterator in combination with RecursiveArrayIterator is to replace array values on a multidimensional array at any level deep.<br><br>Usually, array_walk_recursive would be used to replace values deep within arrays, but unfortunately this only works when there is a standard key value pair - in other words, array_walk_recursive ONLY VISITS LEAF NODES, NOT arrays.<br><br>So to get around this, the iterators can be used in this way:<br><br>

```
<?php
$array = [
    &apos;test&apos; =&gt; &apos;value&apos;,
    &apos;level_one&apos; =&gt; [
        &apos;level_two&apos; =&gt; [
            &apos;level_three&apos; =&gt; [
                &apos;replace_this_array&apos; =&gt; [
                    &apos;special_key&apos; =&gt; &apos;replacement_value&apos;,
                    &apos;key_one&apos; =&gt; &apos;testing&apos;,
                    &apos;key_two&apos; =&gt; &apos;value&apos;,
                    &apos;four&apos; =&gt; &apos;another value&apos;
                ]
            ],
            &apos;ordinary_key&apos; =&gt; &apos;value&apos;
        ]
    ]
];

$arrayIterator = new \RecursiveArrayIterator($array);
$recursiveIterator = new \RecursiveIteratorIterator($arrayIterator, \RecursiveIteratorIterator::SELF_FIRST);

foreach ($recursiveIterator as $key =&gt; $value) {
    if (is_array($value) &amp;&amp; array_key_exists(&apos;special_key&apos;, $value)) {
        // Here we replace ALL keys with the same value from &apos;special_key&apos;
        $replaced = array_fill(0, count($value), $value[&apos;special_key&apos;]);
        $value = array_combine(array_keys($value), $replaced);
        // set a new key
        $value[&apos;new_key&apos;] = &apos;new value&apos;;

        // Get the current depth and traverse back up the tree, saving the modifications
        $currentDepth = $recursiveIterator-&gt;getDepth();
        for ($subDepth = $currentDepth; $subDepth &gt;= 0; $subDepth--) {
            // Get the current level iterator
            $subIterator = $recursiveIterator-&gt;getSubIterator($subDepth); 
            // If we are on the level we want to change, use the replacements ($value) other wise set the key to the parent iterators value
            $subIterator-&gt;offsetSet($subIterator-&gt;key(), ($subDepth === $currentDepth ? $value : $recursiveIterator-&gt;getSubIterator(($subDepth+1))-&gt;getArrayCopy())); 
       }
    }
}
return $recursiveIterator-&gt;getArrayCopy();
// return:
$array = [
    &apos;test&apos; =&gt; &apos;value&apos;,
    &apos;level_one&apos; =&gt; [
        &apos;level_two&apos; =&gt; [
            &apos;level_three&apos; =&gt; [
                &apos;replace_this_array&apos; =&gt; [
                    &apos;special_key&apos; =&gt; &apos;replacement_value&apos;,
                    &apos;key_one&apos; =&gt; &apos;replacement_value&apos;,
                    &apos;key_two&apos; =&gt; &apos;replacement_value&apos;,
                    &apos;four&apos; =&gt; &apos;replacement_value&apos;,
                    &apos;new_key&apos; =&gt; &apos;new value&apos;
                ]
            ],
            &apos;ordinary_key&apos; =&gt; &apos;value&apos;
        ]
    ]
];
?>
```
<br><br>The key is in traversing back up the tree to save the changes at that level - simply calling $recursiveIterator-&gt;offsetSet(); will only set a key on the root array.  

#

This example demonstrates using the getDepth() method with a RecursiveArrayIterator.<br><br>

```
<?php
$tree = array();
$tree[1][2][3] = &apos;lemon&apos;;
$tree[1][4] = &apos;melon&apos;;
$tree[2][3] = &apos;orange&apos;;
$tree[2][5] = &apos;grape&apos;;
$tree[3] = &apos;pineapple&apos;;

print_r($tree);
 
$arrayiter = new RecursiveArrayIterator($tree);
$iteriter = new RecursiveIteratorIterator($arrayiter);
 
foreach ($iteriter as $key =&gt; $value) {
  $d = $iteriter-&gt;getDepth();
  echo "depth=$d k=$key v=$value\n";
}
?>
```
<br><br>The output of this would be:<br><br>Array<br>(<br>    [1] =&gt; Array<br>        (<br>            [2] =&gt; Array<br>                (<br>                    [3] =&gt; lemon<br>                )<br><br>            [4] =&gt; melon<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [3] =&gt; orange<br>            [5] =&gt; grape<br>        )<br><br>    [3] =&gt; pineapple<br>)<br><br>depth=2 k=3 v=lemon<br>depth=1 k=4 v=melon<br>depth=1 k=3 v=orange<br>depth=1 k=5 v=grape<br>depth=0 k=3 v=pineapple  

#

A very important thing to note about \RecursiveIteratorIterator is that it returns a flattened array when used with the iterator_to_array function. Ex:<br><br>

```
<?php
$arr = array(&apos;Zero&apos;, &apos;name&apos;=&gt;&apos;Adil&apos;, &apos;address&apos; =&gt; array( &apos;city&apos;=&gt;&apos;Dubai&apos;, &apos;tel&apos; =&gt; array(&apos;int&apos; =&gt; 971, &apos;tel&apos;=&gt;12345487)), &apos;&apos; =&gt; &apos;nothing&apos;);

$iterator = new \RecursiveIteratorIterator(new \RecursiveArrayIterator($arr));
var_dump(iterator_to_array($iterator,true));
?>
```
<br><br>This code will return :<br><br>array(6) {<br>  [0]=&gt;<br>  string(4) "Zero"<br>  ["name"]=&gt;<br>  string(4) "Adil"<br>  ["city"]=&gt;<br>  string(5) "Dubai"<br>  ["int"]=&gt;<br>  int(91)<br>  ["tel"]=&gt;<br>  int(12345487)<br>  [""]=&gt;<br>  string(7) "nothing"<br>}<br><br>To get the non-flattened proper array use the getArrayCopy() method, like so :<br><br>$iterator-&gt;getArrayCopy()<br><br>This will return<br><br>array(4) {<br>  [0]=&gt;<br>  string(4) "Zero"<br>  ["name"]=&gt;<br>  string(4) "Adil"<br>  ["address"]=&gt;<br>  array(2) {<br>    ["city"]=&gt;<br>    string(5) "Dubai"<br>    ["tel"]=&gt;<br>    array(2) {<br>      ["int"]=&gt;<br>      int(91)<br>      ["tel"]=&gt;<br>      int(12345487)<br>    }<br>  }<br>  [""]=&gt;<br>  string(7) "nothing"<br>}  

#

You can use this to quickly find all the files (recursively) in a certain directory. This beats maintaining a stack yourself.<br>

```
<?php
$directory = "/tmp/";
$fileSPLObjects =  new RecursiveIteratorIterator(
                new RecursiveDirectoryIterator($directory),
                RecursiveIteratorIterator::CHILD_FIRST
            );
try {
    foreach( $fileSPLObjects as $fullFileName =&gt; $fileSPLObject ) {
        print $fullFileName . " " . $fileSPLObject-&gt;getFilename() . "\n";
    }
}
catch (UnexpectedValueException $e) {
    printf("Directory [%s] contained a directory we can not recurse into", $directory);
}
?>
```
<br>Note: if there is a directory contained within the directory you are searching in that you have no access to read an UnexpectedValueException will be thrown (leaving you with an empty list).<br>Note: objects returned are SPLFileObjects  

#

[Official documentation page](https://www.php.net/manual/en/class.recursiveiteratoriterator.php)

**[To root](/README.md)**