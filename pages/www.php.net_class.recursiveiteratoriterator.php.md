# The RecursiveIteratorIterator class



Some speed tests<br>

```
<?php
$timer = function ($name = 'default', $unset_timer = TRUE)
{
    static $timers = array();
    
    if ( isset( $timers[ $name ] ) )
    {
        list($s_sec, $s_mic) = explode(' ', $timers[ $name ]);
        list($e_sec, $e_mic) = explode(' ', microtime());
        
        if ( $unset_timer )
            unset( $timers[ $name ] );
        
        return $e_sec - $s_sec + ( $e_mic - $s_mic );
    }
    
    $timers[ $name ] = microtime();
};

function f1 ($array) {
    $iterator = new RecursiveIteratorIterator(new RecursiveArrayIterator($array), RecursiveIteratorIterator::SELF_FIRST);

    foreach ( $iterator as $key => $value ) {
        if ( is_array($value) )
            continue;
    }
}

function f2($array) {
    foreach ( $array as $key => $value ) {
        if ( is_array($value) )
            f2($value);
    }
}

foreach ( [100, 1000, 10000, 100000, 1000000] as $num )
{
    $array = [];
    
    for ( $i = 0; ++$i < $num; )
        $array[] = [1,2,3=>[4,5,6=>[7,8,9=>10,11,12=>[13,14,15=>[16,17,18]]]]];
    
    $timer();
    f1($array);
    printf("RecursiveIteratorIterator: %7d elements -> %.3f sec\n", $num, $timer());
    
    $timer();
    f2($array);
    printf("Recursive function       : %7d elements -> %.3f sec\n", $num, $timer());
}

?>
```
<br><br>Output (PHP 5.4.9-4ubuntu2.1 (cli) (built: Jun 11 2013 13:10:01))<br>=======================<br>RecursiveIteratorIterator:     100 elements -&gt; 0.007 sec<br>Recursive function       :     100 elements -&gt; 0.002 sec<br>RecursiveIteratorIterator:    1000 elements -&gt; 0.036 sec<br>Recursive function       :    1000 elements -&gt; 0.024 sec<br>RecursiveIteratorIterator:   10000 elements -&gt; 0.425 sec<br>Recursive function       :   10000 elements -&gt; 0.263 sec<br>RecursiveIteratorIterator:  100000 elements -&gt; 8.153 sec<br>Recursive function       :  100000 elements -&gt; 2.654 sec<br>RecursiveIteratorIterator: 1000000 elements -&gt; 474.483 sec<br>Recursive function       : 1000000 elements -&gt; 26.872 sec<br><br>For one million elements recursive function is more quickly!  

#

A very useful use case for RecusiveIteratorIterator in combination with RecursiveArrayIterator is to replace array values on a multidimensional array at any level deep.<br><br>Usually, array_walk_recursive would be used to replace values deep within arrays, but unfortunately this only works when there is a standard key value pair - in other words, array_walk_recursive ONLY VISITS LEAF NODES, NOT arrays.<br><br>So to get around this, the iterators can be used in this way:<br><br>

```
<?php
$array = [
    'test' => 'value',
    'level_one' => [
        'level_two' => [
            'level_three' => [
                'replace_this_array' => [
                    'special_key' => 'replacement_value',
                    'key_one' => 'testing',
                    'key_two' => 'value',
                    'four' => 'another value'
                ]
            ],
            'ordinary_key' => 'value'
        ]
    ]
];

$arrayIterator = new \RecursiveArrayIterator($array);
$recursiveIterator = new \RecursiveIteratorIterator($arrayIterator, \RecursiveIteratorIterator::SELF_FIRST);

foreach ($recursiveIterator as $key => $value) {
    if (is_array($value) &amp;&amp; array_key_exists('special_key', $value)) {
        // Here we replace ALL keys with the same value from 'special_key'
        $replaced = array_fill(0, count($value), $value['special_key']);
        $value = array_combine(array_keys($value), $replaced);
        // set a new key
        $value['new_key'] = 'new value';

        // Get the current depth and traverse back up the tree, saving the modifications
        $currentDepth = $recursiveIterator->getDepth();
        for ($subDepth = $currentDepth; $subDepth >= 0; $subDepth--) {
            // Get the current level iterator
            $subIterator = $recursiveIterator->getSubIterator($subDepth); 
            // If we are on the level we want to change, use the replacements ($value) other wise set the key to the parent iterators value
            $subIterator->offsetSet($subIterator->key(), ($subDepth === $currentDepth ? $value : $recursiveIterator->getSubIterator(($subDepth+1))->getArrayCopy())); 
       }
    }
}
return $recursiveIterator->getArrayCopy();
// return:
$array = [
    'test' => 'value',
    'level_one' => [
        'level_two' => [
            'level_three' => [
                'replace_this_array' => [
                    'special_key' => 'replacement_value',
                    'key_one' => 'replacement_value',
                    'key_two' => 'replacement_value',
                    'four' => 'replacement_value',
                    'new_key' => 'new value'
                ]
            ],
            'ordinary_key' => 'value'
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
$tree[1][2][3] = 'lemon';
$tree[1][4] = 'melon';
$tree[2][3] = 'orange';
$tree[2][5] = 'grape';
$tree[3] = 'pineapple';

print_r($tree);
 
$arrayiter = new RecursiveArrayIterator($tree);
$iteriter = new RecursiveIteratorIterator($arrayiter);
 
foreach ($iteriter as $key => $value) {
  $d = $iteriter->getDepth();
  echo "depth=$d k=$key v=$value\n";
}
?>
```
<br><br>The output of this would be:<br><br>Array<br>(<br>    [1] =&gt; Array<br>        (<br>            [2] =&gt; Array<br>                (<br>                    [3] =&gt; lemon<br>                )<br><br>            [4] =&gt; melon<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [3] =&gt; orange<br>            [5] =&gt; grape<br>        )<br><br>    [3] =&gt; pineapple<br>)<br><br>depth=2 k=3 v=lemon<br>depth=1 k=4 v=melon<br>depth=1 k=3 v=orange<br>depth=1 k=5 v=grape<br>depth=0 k=3 v=pineapple  

#

A very important thing to note about \RecursiveIteratorIterator is that it returns a flattened array when used with the iterator_to_array function. Ex:<br><br>

```
<?php
$arr = array('Zero', 'name'=>'Adil', 'address' => array( 'city'=>'Dubai', 'tel' => array('int' => 971, 'tel'=>12345487)), '' => 'nothing');

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
    foreach( $fileSPLObjects as $fullFileName => $fileSPLObject ) {
        print $fullFileName . " " . $fileSPLObject->getFilename() . "\n";
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