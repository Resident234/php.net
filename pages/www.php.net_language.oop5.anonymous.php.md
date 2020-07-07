# Anonymous classes





Below three examples describe anonymous class with very simple and basic but quite understandable example



```
<?php
// First way - anonymous class assigned directly to variable
$ano_class_obj = new class{
&#xA0; &#xA0; public $prop1 = &apos;hello&apos;;
&#xA0; &#xA0; public $prop2 = 754;
&#xA0; &#xA0; const SETT = &apos;some config&apos;;

&#xA0; &#xA0; public function getValue()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // do some operation
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;some returned value&apos;;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function getValueWithArgu($str)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // do some operation
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;returned value is &apos;.$str;
&#xA0; &#xA0; }
};

echo &quot;\n&quot;;

var_dump($ano_class_obj);
echo &quot;\n&quot;;

echo $ano_class_obj-&gt;prop1;
echo &quot;\n&quot;;

echo $ano_class_obj-&gt;prop2;
echo &quot;\n&quot;;

echo $ano_class_obj::SETT;
echo &quot;\n&quot;;

echo $ano_class_obj-&gt;getValue();
echo &quot;\n&quot;;

echo $ano_class_obj-&gt;getValueWithArgu(&apos;OOP&apos;);
echo &quot;\n&quot;;

echo &quot;\n&quot;;

// Second way - anonymous class assigned to variable via defined function
$ano_class_obj_with_func = ano_func();

function ano_func()
{
&#xA0; &#xA0; return new class {
&#xA0; &#xA0; &#xA0; &#xA0; public $prop1 = &apos;hello&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; public $prop2 = 754;
&#xA0; &#xA0; &#xA0; &#xA0; const SETT = &apos;some config&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; public function getValue()
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // do some operation
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;some returned value&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function getValueWithArgu($str)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // do some operation
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;returned value is &apos;.$str;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; };
}

echo &quot;\n&quot;;

var_dump($ano_class_obj_with_func);
echo &quot;\n&quot;;

echo $ano_class_obj_with_func-&gt;prop1;
echo &quot;\n&quot;;

echo $ano_class_obj_with_func-&gt;prop2;
echo &quot;\n&quot;;

echo $ano_class_obj_with_func::SETT;
echo &quot;\n&quot;;

echo $ano_class_obj_with_func-&gt;getValue();
echo &quot;\n&quot;;

echo $ano_class_obj_with_func-&gt;getValueWithArgu(&apos;OOP&apos;);
echo &quot;\n&quot;;

echo &quot;\n&quot;;

// Third way - passing argument to anonymous class via constructors
$arg = 1; // we got it by some operation
$config = [2, false]; // we got it by some operation
$ano_class_obj_with_arg = ano_func_with_arg($arg, $config);

function ano_func_with_arg($arg, $config)
{
&#xA0; &#xA0; return new class($arg, $config) {
&#xA0; &#xA0; &#xA0; &#xA0; public $prop1 = &apos;hello&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; public $prop2 = 754;
&#xA0; &#xA0; &#xA0; &#xA0; public $prop3, $config;
&#xA0; &#xA0; &#xA0; &#xA0; const SETT = &apos;some config&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; public function __construct($arg, $config)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;prop3 = $arg;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;config =$config;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function getValue()
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // do some operation
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;some returned value&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function getValueWithArgu($str)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // do some operation
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;returned value is &apos;.$str;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; };
}

echo &quot;\n&quot;;

var_dump($ano_class_obj_with_arg);
echo &quot;\n&quot;;

echo $ano_class_obj_with_arg-&gt;prop1;
echo &quot;\n&quot;;

echo $ano_class_obj_with_arg-&gt;prop2;
echo &quot;\n&quot;;

echo $ano_class_obj_with_arg::SETT;
echo &quot;\n&quot;;

echo $ano_class_obj_with_arg-&gt;getValue();
echo &quot;\n&quot;;

echo $ano_class_obj_with_arg-&gt;getValueWithArgu(&apos;OOP&apos;);
echo &quot;\n&quot;;

echo &quot;\n&quot;;


  

#



Anonymous classes are syntax sugar that may appear deceiving to some.
The &apos;anonymous&apos; class is still parsed into the global scope, where it is auto assigned a name, and every time the class is needed, that global class definition is used.&#xA0; Example to illustrate....

The anonymous class version...


```
<?php

function return_anon(){
&#xA0; &#xA0; return new class{
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public static $str=&quot;foo&quot;;&#xA0; 
&#xA0; &#xA0; };
}
$test=return_anon();
echo $test::$str; //ouputs foo

//we can still access the &apos;anon&apos; class directly in the global scope! 
$another=get_class($test); //get the auto assigned name
echo $another::$str;&#xA0; &#xA0; //outputs foo
?>
```


The above is functionally the same as doing this....


```
<?php
class I_named_this_one{
&#xA0; &#xA0; public static $str=&quot;foo&quot;;
}
function return_not_anon(){
&#xA0; &#xA0; return &apos;I_named_this_one&apos;;
}
$clzz=return_not_anon();//get class name
echo $clzz::$str;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.anonymous.php)

**[To root](/README.md)**