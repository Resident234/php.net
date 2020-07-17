# get_object_vars



You can still cast the object to an array to get all its members and see its visibility. Example:<br><br>

```
<?php

class Potatoe {
    public $skin;
    protected $meat;
    private $roots;

    function __construct ( $s, $m, $r ) {
        $this->skin = $s;
        $this->meat = $m;
        $this->roots = $r;
    }
}

$Obj = new Potatoe ( 1, 2, 3 );

echo "<pre>\n";
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
    [skin] => 1
)

Using array cast:
Array
(
    [skin] => 1
    [ * meat] => 2
    [ Potatoe roots] => 3
)

As you can see, you can obtain the visibility for each member from this cast. That which seems to be spaces into array keys are '\0' characters, so the general rule to parse keys seems to be:

Public members: member_name
Protected memebers: \0*\0member_name
Private members: \0Class_name\0member_name

I've wroten a obj2array function that creates entries without visibility for each key, so you can handle them into the array as it were within the object:



```
<?php

function obj2array ( &amp;$Instance ) {
    $clone = (array) $Instance;
    $rtn = array ();
    $rtn['___SOURCE_KEYS_'] = $clone;

    while ( list ($key, $value) = each ($clone) ) {
        $aux = explode ("\0", $key);
        $newkey = $aux[count($aux)-1];
        $rtn[$newkey] = &amp;$rtn['___SOURCE_KEYS_'][$key];
    }

    return $rtn;
}

?>
```


I've created also a <i>bless</i> function that works similar to Perl's bless, so you can further recast the array converting it in an object of an specific class:



```
<?php

function bless ( &amp;$Instance, $Class ) {
    if ( ! (is_array ($Instance) ) ) {
        return NULL;
    }

    // First get source keys if available
    if ( isset ($Instance['___SOURCE_KEYS_'])) {
        $Instance = $Instance['___SOURCE_KEYS_'];
    }

    // Get serialization data from array
    $serdata = serialize ( $Instance );

    list ($array_params, $array_elems) = explode ('{', $serdata, 2);
    list ($array_tag, $array_count) = explode (':', $array_params, 3 );
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
        $this->skin = $s;
        $this->meat = $m;
        $this->roots = $r;
    }

    function PrintAll () {
        echo "skin = ".$this->skin."\n";
        echo "meat = ".$this->meat."\n";
        echo "roots = ".$this->roots."\n";
    }
}

$Obj = new Potatoe ( 1, 2, 3 );

echo "<pre>\n";
echo "Using get_object_vars:\n";

$vars = get_object_vars ( $Obj );
print_r ( $vars );

echo "\n\nUsing obj2array func:\n";

$Arr = obj2array($Obj);
print_r ( $Arr );

echo "\n\nSetting all members to 0.\n";
$Arr['skin']=0;
$Arr['meat']=0;
$Arr['roots']=0;

echo "Converting the array into an instance of the original class.\n";
bless ( $Arr, Potatoe );

if ( is_object ($Arr) ) {
    echo "\$Arr is now an object.\n";
    if ( $Arr instanceof Potatoe ) {
        echo "\$Arr is an instance of Potatoe class.\n";
    }
}

$Arr->PrintAll();

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
        foreach ($_arr as $key => $val) {
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
    [Fruits] => Array
        (
            [0] => fruits Object
                (
                    [_name] => Mango
                    [_color] => Green
                    [_weight] => 10
                )

            [1] => fruits Object
                (
                    [_name] => Apple
                    [_color] => Red
                    [_weight] => 15
                )

            [2] => fruits Object
                (
                    [_name] => Grape
                    [_color] => Purple
                    [_weight] => 5
                )

        )

    [total_weight] => 30
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