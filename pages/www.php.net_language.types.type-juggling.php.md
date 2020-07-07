# Type Juggling





Uneven division of an integer variable by another integer variable will result in a float by automatic conversion -- you do not have to cast the variables to floats in order to avoid integer truncation (as you would in C, for example):

$dividend = 2;
$divisor = 3;
$quotient = $dividend/$divisor;
print $quotient; // 0.66666666666667

  

#



&quot;An example of PHP&apos;s automatic type conversion is the multiplication operator &apos;*&apos;. If either operand is a float, then both operands are evaluated as floats, and the result will be a float. Otherwise, the operands will be interpreted as integers, and the result will also be an integer. Note that this does not change the types of the operands themselves; the only change is in how the operands are evaluated and what the type of the expression itself is.&quot;

I understand what the doc is trying to say here, but this sentence is not correct as stated, other types can be coerced into floats.

e.g.



```
<?php
$a = &quot;1.5&quot;; // $a is a string
$b = 100; // $b is an int
$c = $a * $b; // $c is a float, value is 150
// multiplication resulted in a float despite fact that neither operand was a float


  

#



incremental operator (&quot;++&quot;) doesn&apos;t make type conversion from boolean to int, and if an variable is boolean and equals TRUE than after ++ operation it remains as TRUE, so:

$a = TRUE; 
echo ($a++).$a;&#xA0; // prints &quot;11&quot;

  

#



Casting objects to arrays is a pain. Example:



```
<?php

class MyClass {

&#xA0; &#xA0; private $priv = &apos;priv_value&apos;;
&#xA0; &#xA0; protected $prot = &apos;prot_value&apos;;
&#xA0; &#xA0; public $pub = &apos;pub_value&apos;;
&#xA0; &#xA0; public $MyClasspriv = &apos;second_pub_value&apos;;

}

$test = new MyClass();
echo &apos;&lt;pre&gt;&apos;;
print_r((array) $test);

/*
Array
(
&#xA0; &#xA0; [MyClasspriv] =&gt; priv_value
&#xA0; &#xA0; [*prot] =&gt; prot_value
&#xA0; &#xA0; [pub] =&gt; pub_value
&#xA0; &#xA0; [MyClasspriv] =&gt; second_pub_value
)
 */

?>
```


Yes, that looks like an array with two keys with the same name and it looks like the protected field was prepended with an asterisk. But that&apos;s not true:



```
<?php

foreach ((array) $test as $key =&gt; $value) {
&#xA0; &#xA0; $len = strlen($key);
&#xA0; &#xA0; echo &quot;{$key} ({$len}) =&gt; {$value}&lt;br /&gt;&quot;;
&#xA0; &#xA0; for ($i = 0; $i &lt; $len; ++$i) {
&#xA0; &#xA0; &#xA0; &#xA0; echo ord($key[$i]) . &apos; &apos;;
&#xA0; &#xA0; }
&#xA0; &#xA0; echo &apos;&lt;hr /&gt;&apos;;
}

/*
MyClasspriv (13) =&gt; priv_value
0 77 121 67 108 97 115 115 0 112 114 105 118
*prot (7) =&gt; prot_value
0 42 0 112 114 111 116
pub (3) =&gt; pub_value
112 117 98
MyClasspriv (11) =&gt; second_pub_value
77 121 67 108 97 115 115 112 114 105 118
 */

?>
```


The char codes show that the protected keys are prepended with &apos;\0*\0&apos; and private keys are prepended with &apos;\0&apos;.__CLASS__.&apos;\0&apos; so be careful when playing around with this.

  

#



Printing or echoing a FALSE boolean value or a NULL value results in an empty string:
(string)TRUE //returns &quot;1&quot;
(string)FALSE //returns &quot;&quot;
echo TRUE; //prints &quot;1&quot;
echo FALSE; //prints nothing!

  

#



The object casting methods presented here do not take into account the class hierarchy of the class you&apos;re trying to cast your object into.

/**
&#xA0; &#xA0;&#xA0; * Convert an object to a specific class.
&#xA0; &#xA0;&#xA0; * @param object $object
&#xA0; &#xA0;&#xA0; * @param string $class_name The class to cast the object to
&#xA0; &#xA0;&#xA0; * @return object
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public static function cast($object, $class_name) {
&#xA0; &#xA0; &#xA0; &#xA0; if($object === false) return false;
&#xA0; &#xA0; &#xA0; &#xA0; if(class_exists($class_name)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ser_object&#xA0; &#xA0;&#xA0; = serialize($object);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $obj_name_len&#xA0; &#xA0;&#xA0; = strlen(get_class($object));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $start&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = $obj_name_len + strlen($obj_name_len) + 6;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new_object&#xA0; &#xA0; &#xA0; = &apos;O:&apos; . strlen($class_name) . &apos;:&quot;&apos; . $class_name . &apos;&quot;:&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new_object&#xA0; &#xA0;&#xA0; .= substr($ser_object, $start);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new_object&#xA0; &#xA0;&#xA0; = unserialize($new_object);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /**
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * The new object is of the correct type but
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * is not fully initialized throughout its graph.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * To get the full object graph (including parent
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * class data, we need to create a new instance of 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * the specified class and then assign the new 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * properties to it.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $graph = new $class_name;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($new_object as $prop =&gt; $val) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $graph-&gt;$prop = $val;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $graph;
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new CoreException(false, &quot;could not find class $class_name for casting in DB::cast&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

  

#



There are some shorter and faster (at least on my machine) ways to perform a type cast.


```
<?php
$string=&apos;12345.678&apos;;
$float=+$string; 
$integer=0|$string;
$boolean=!!$string;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.types.type-juggling.php)

**[To root](/README.md)**