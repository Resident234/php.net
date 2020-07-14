# Type Juggling



Uneven division of an integer variable by another integer variable will result in a float by automatic conversion -- you do not have to cast the variables to floats in order to avoid integer truncation (as you would in C, for example):<br><br>$dividend = 2;<br>$divisor = 3;<br>$quotient = $dividend/$divisor;<br>print $quotient; // 0.66666666666667  

#

"An example of PHP&apos;s automatic type conversion is the multiplication operator &apos;*&apos;. If either operand is a float, then both operands are evaluated as floats, and the result will be a float. Otherwise, the operands will be interpreted as integers, and the result will also be an integer. Note that this does not change the types of the operands themselves; the only change is in how the operands are evaluated and what the type of the expression itself is."<br><br>I understand what the doc is trying to say here, but this sentence is not correct as stated, other types can be coerced into floats.<br><br>e.g.<br><br>

```
<?php
$a = "1.5"; // $a is a string
$b = 100; // $b is an int
$c = $a * $b; // $c is a float, value is 150
// multiplication resulted in a float despite fact that neither operand was a float?>
```
  

#

incremental operator ("++") doesn&apos;t make type conversion from boolean to int, and if an variable is boolean and equals TRUE than after ++ operation it remains as TRUE, so:<br><br>$a = TRUE; <br>echo ($a++).$a;  // prints "11"  

#

Casting objects to arrays is a pain. Example:<br><br>

```
<?php

class MyClass {

    private $priv = 'priv_value';
    protected $prot = 'prot_value';
    public $pub = 'pub_value';
    public $MyClasspriv = 'second_pub_value';

}

$test = new MyClass();
echo '&lt;pre&gt;';
print_r((array) $test);

/*
Array
(
    [MyClasspriv] => priv_value
    [*prot] => prot_value
    [pub] => pub_value
    [MyClasspriv] => second_pub_value
)
 */

?>
```


Yes, that looks like an array with two keys with the same name and it looks like the protected field was prepended with an asterisk. But that's not true:



```
<?php

foreach ((array) $test as $key => $value) {
    $len = strlen($key);
    echo "{$key} ({$len}) => {$value}&lt;br /&gt;";
    for ($i = 0; $i &lt; $len; ++$i) {
        echo ord($key[$i]) . ' ';
    }
    echo '&lt;hr /&gt;';
}

/*
MyClasspriv (13) => priv_value
0 77 121 67 108 97 115 115 0 112 114 105 118
*prot (7) => prot_value
0 42 0 112 114 111 116
pub (3) => pub_value
112 117 98
MyClasspriv (11) => second_pub_value
77 121 67 108 97 115 115 112 114 105 118
 */

?>
```
<br><br>The char codes show that the protected keys are prepended with &apos;\0*\0&apos; and private keys are prepended with &apos;\0&apos;.__CLASS__.&apos;\0&apos; so be careful when playing around with this.  

#

Printing or echoing a FALSE boolean value or a NULL value results in an empty string:<br>(string)TRUE //returns "1"<br>(string)FALSE //returns ""<br>echo TRUE; //prints "1"<br>echo FALSE; //prints nothing!  

#

The object casting methods presented here do not take into account the class hierarchy of the class you&apos;re trying to cast your object into.<br><br>/**<br>     * Convert an object to a specific class.<br>     * @param object $object<br>     * @param string $class_name The class to cast the object to<br>     * @return object<br>     */<br>    public static function cast($object, $class_name) {<br>        if($object === false) return false;<br>        if(class_exists($class_name)) {<br>            $ser_object     = serialize($object);<br>            $obj_name_len     = strlen(get_class($object));<br>            $start             = $obj_name_len + strlen($obj_name_len) + 6;<br>            $new_object      = &apos;O:&apos; . strlen($class_name) . &apos;:"&apos; . $class_name . &apos;":&apos;;<br>            $new_object     .= substr($ser_object, $start);<br>            $new_object     = unserialize($new_object);<br>            /**<br>             * The new object is of the correct type but<br>             * is not fully initialized throughout its graph.<br>             * To get the full object graph (including parent<br>             * class data, we need to create a new instance of <br>             * the specified class and then assign the new <br>             * properties to it.<br>             */<br>            $graph = new $class_name;<br>            foreach($new_object as $prop =&gt; $val) {<br>                $graph-&gt;$prop = $val;<br>            }<br>            return $graph;<br>        } else {<br>            throw new CoreException(false, "could not find class $class_name for casting in DB::cast");<br>            return false;<br>        }<br>    }  

#

There are some shorter and faster (at least on my machine) ways to perform a type cast.<br>

```
<?php
$string='12345.678';
$float=+$string; 
$integer=0|$string;
$boolean=!!$string;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.type-juggling.php)

**[To root](/README.md)**