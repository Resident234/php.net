# get_object_vars



You can still cast the object to an array to get all its members and see its visibility. Example:<br><br>

```
<?php

class Potatoe {
    public $skin;
    protected $meat;
    private $roots;

    function __construct ( $s, $m, $r ) {
        $this-&gt;skin = $s;
        $this-&gt;meat = $m;
        $this-&gt;roots = $r;
    }
}

$Obj = new Potatoe ( 1, 2, 3 );

echo "&lt;pre&gt;\n";
echo "Using get_object_vars:\n";

$vars = get_object_vars ( $Obj );
print_r ( $vars );

echo "\n\nUsing array cast:\n";

$Arr = (array)$Obj;
print_r ( $Arr );

?>
```


This will returns:

Using get_object_vars:
Array
(
    [skin] =&gt; 1
)

Using array cast:
Array
(
    [skin] =&gt; 1
    [ * meat] =&gt; 2
    [ Potatoe roots] =&gt; 3
)

As you can see, you can obtain the visibility for each member from this cast. That which seems to be spaces into array keys are &apos;\0&apos; characters, so the general rule to parse keys seems to be:

Public members: member_name
Protected memebers: \0*\0member_name
Private members: \0Class_name\0member_name

I&apos;ve wroten a obj2array function that creates entries without visibility for each key, so you can handle them into the array as it were within the object:



```
<?php

function obj2array ( &amp;$Instance ) {
    $clone = (array) $Instance;
    $rtn = array ();
    $rtn[&apos;___SOURCE_KEYS_&apos;] = $clone;

    while ( list ($key, $value) = each ($clone) ) {
        $aux = explode ("\0", $key);
        $newkey = $aux[count($aux)-1];
        $rtn[$newkey] = &amp;$rtn[&apos;___SOURCE_KEYS_&apos;][$key];
    }

    return $rtn;
}

?>
```


I&apos;ve created also a &lt;i&gt;bless&lt;/i&gt; function that works similar to Perl&apos;s bless, so you can further recast the array converting it in an object of an specific class:



```
<?php

function bless ( &amp;$Instance, $Class ) {
    if ( ! (is_array ($Instance) ) ) {
        return NULL;
    }

    // First get source keys if available
    if ( isset ($Instance[&apos;___SOURCE_KEYS_&apos;])) {
        $Instance = $Instance[&apos;___SOURCE_KEYS_&apos;];
    }

    // Get serialization data from array
    $serdata = serialize ( $Instance );

    list ($array_params, $array_elems) = explode (&apos;{&apos;, $serdata, 2);
    list ($array_tag, $array_count) = explode (&apos;:&apos;, $array_params, 3 );
    $serdata = "O:".strlen ($Class).":\"$Class\":$array_count:{".$array_elems;

    $Instance = unserialize ( $serdata );
    return $Instance;
}
?>
```


With these ones you can do things like:



```
<?php

define("SFCMS_DIR", dirname(__FILE__)."/..");
require_once (SFCMS_DIR."/Misc/bless.php");

class Potatoe {
    public $skin;
    protected $meat;
    private $roots;

    function __construct ( $s, $m, $r ) {
        $this-&gt;skin = $s;
        $this-&gt;meat = $m;
        $this-&gt;roots = $r;
    }

    function PrintAll () {
        echo "skin = ".$this-&gt;skin."\n";
        echo "meat = ".$this-&gt;meat."\n";
        echo "roots = ".$this-&gt;roots."\n";
    }
}

$Obj = new Potatoe ( 1, 2, 3 );

echo "&lt;pre&gt;\n";
echo "Using get_object_vars:\n";

$vars = get_object_vars ( $Obj );
print_r ( $vars );

echo "\n\nUsing obj2array func:\n";

$Arr = obj2array($Obj);
print_r ( $Arr );

echo "\n\nSetting all members to 0.\n";
$Arr[&apos;skin&apos;]=0;
$Arr[&apos;meat&apos;]=0;
$Arr[&apos;roots&apos;]=0;

echo "Converting the array into an instance of the original class.\n";
bless ( $Arr, Potatoe );

if ( is_object ($Arr) ) {
    echo "\$Arr is now an object.\n";
    if ( $Arr instanceof Potatoe ) {
        echo "\$Arr is an instance of Potatoe class.\n";
    }
}

$Arr-&gt;PrintAll();

?>
```
  

#

You can use call_user_func() to return public variables from inside the class:<br><br>class Test {<br>    protected $protected;<br>    public $public;<br>    private $private;<br><br>    public function getPublicVars () {<br>        return call_user_func(&apos;get_object_vars&apos;, $this);<br>    }<br>}<br><br>$test = new Test();<br>var_dump($test-&gt;getPublicVars()); // array("public" =&gt; NULL)  

#

Hi all, I just wrote a function which dumps all the object propreties and its associations recursively into an array. Here it is..<br>

```
<?php
function object_to_array($obj) {
        $_arr = is_object($obj) ? get_object_vars($obj) : $obj;
        foreach ($_arr as $key =&gt; $val) {
                $val = (is_array($val) || is_object($val)) ? object_to_array($val) : $val;
                $arr[$key] = $val;
        }
        return $arr;
}
?>
```


Example:
You have an object like this:
fruitsbasket Object
(
    [Fruits] =&gt; Array
        (
            [0] =&gt; fruits Object
                (
                    [_name] =&gt; Mango
                    [_color] =&gt; Green
                    [_weight] =&gt; 10
                )

            [1] =&gt; fruits Object
                (
                    [_name] =&gt; Apple
                    [_color] =&gt; Red
                    [_weight] =&gt; 15
                )

            [2] =&gt; fruits Object
                (
                    [_name] =&gt; Grape
                    [_color] =&gt; Purple
                    [_weight] =&gt; 5
                )

        )

    [total_weight] =&gt; 30
)

just do:


```
<?php
$the_array = object_to_array($the_object);
print_r($the_array);
?>
```
<br><br>it will produce an array:<br>Array<br>(<br>    [Fruits] =&gt; Array<br>        (<br>            [0] =&gt; Array<br>                (<br>                    [_name] =&gt; Mango<br>                    [_color] =&gt; Green<br>                    [_weight] =&gt; 10<br>                )<br><br>            [1] =&gt; Array<br>                (<br>                    [_name] =&gt; Apple<br>                    [_color] =&gt; Red<br>                    [_weight] =&gt; 15<br>                )<br><br>            [2] =&gt; Array<br>                (<br>                    [_name] =&gt; Grape<br>                    [_color] =&gt; Purple<br>                    [_weight] =&gt; 5<br>                )<br><br>        )<br><br>    [total_weight] =&gt; 30<br>)<br><br>I wish function like this could be usefull for you all. :)  

#

[Official documentation page](https://www.php.net/manual/en/function.get-object-vars.php)

**[To root](/README.md)**