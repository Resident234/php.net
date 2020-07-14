# Anonymous classes



Below three examples describe anonymous class with very simple and basic but quite understandable example<br><br>

```
<?php<br>// First way - anonymous class assigned directly to variable<br>$ano_class_obj = new class{<br>    public $prop1 = &apos;hello&apos;;<br>    public $prop2 = 754;<br>    const SETT = &apos;some config&apos;;<br><br>    public function getValue()<br>    {<br>        // do some operation<br>        return &apos;some returned value&apos;;<br>    }<br><br>    public function getValueWithArgu($str)<br>    {<br>        // do some operation<br>        return &apos;returned value is &apos;.$str;<br>    }<br>};<br><br>echo "\n";<br><br>var_dump($ano_class_obj);<br>echo "\n";<br><br>echo $ano_class_obj-&gt;prop1;<br>echo "\n";<br><br>echo $ano_class_obj-&gt;prop2;<br>echo "\n";<br><br>echo $ano_class_obj::SETT;<br>echo "\n";<br><br>echo $ano_class_obj-&gt;getValue();<br>echo "\n";<br><br>echo $ano_class_obj-&gt;getValueWithArgu(&apos;OOP&apos;);<br>echo "\n";<br><br>echo "\n";<br><br>// Second way - anonymous class assigned to variable via defined function<br>$ano_class_obj_with_func = ano_func();<br><br>function ano_func()<br>{<br>    return new class {<br>        public $prop1 = &apos;hello&apos;;<br>        public $prop2 = 754;<br>        const SETT = &apos;some config&apos;;<br><br>        public function getValue()<br>        {<br>            // do some operation<br>            return &apos;some returned value&apos;;<br>        }<br><br>        public function getValueWithArgu($str)<br>        {<br>            // do some operation<br>            return &apos;returned value is &apos;.$str;<br>        }<br>    };<br>}<br><br>echo "\n";<br><br>var_dump($ano_class_obj_with_func);<br>echo "\n";<br><br>echo $ano_class_obj_with_func-&gt;prop1;<br>echo "\n";<br><br>echo $ano_class_obj_with_func-&gt;prop2;<br>echo "\n";<br><br>echo $ano_class_obj_with_func::SETT;<br>echo "\n";<br><br>echo $ano_class_obj_with_func-&gt;getValue();<br>echo "\n";<br><br>echo $ano_class_obj_with_func-&gt;getValueWithArgu(&apos;OOP&apos;);<br>echo "\n";<br><br>echo "\n";<br><br>// Third way - passing argument to anonymous class via constructors<br>$arg = 1; // we got it by some operation<br>$config = [2, false]; // we got it by some operation<br>$ano_class_obj_with_arg = ano_func_with_arg($arg, $config);<br><br>function ano_func_with_arg($arg, $config)<br>{<br>    return new class($arg, $config) {<br>        public $prop1 = &apos;hello&apos;;<br>        public $prop2 = 754;<br>        public $prop3, $config;<br>        const SETT = &apos;some config&apos;;<br><br>        public function __construct($arg, $config)<br>        {<br>            $this-&gt;prop3 = $arg;<br>            $this-&gt;config =$config;<br>        }<br><br>        public function getValue()<br>        {<br>            // do some operation<br>            return &apos;some returned value&apos;;<br>        }<br><br>        public function getValueWithArgu($str)<br>        {<br>            // do some operation<br>            return &apos;returned value is &apos;.$str;<br>        }<br>    };<br>}<br><br>echo "\n";<br><br>var_dump($ano_class_obj_with_arg);<br>echo "\n";<br><br>echo $ano_class_obj_with_arg-&gt;prop1;<br>echo "\n";<br><br>echo $ano_class_obj_with_arg-&gt;prop2;<br>echo "\n";<br><br>echo $ano_class_obj_with_arg::SETT;<br>echo "\n";<br><br>echo $ano_class_obj_with_arg-&gt;getValue();<br>echo "\n";<br><br>echo $ano_class_obj_with_arg-&gt;getValueWithArgu(&apos;OOP&apos;);<br>echo "\n";<br><br>echo "\n";  

#

Anonymous classes are syntax sugar that may appear deceiving to some.<br>The &apos;anonymous&apos; class is still parsed into the global scope, where it is auto assigned a name, and every time the class is needed, that global class definition is used.  Example to illustrate....<br><br>The anonymous class version...<br>

```
<?php

function return_anon(){
    return new class{
         public static $str="foo";  
    };
}
$test=return_anon();
echo $test::$str; //ouputs foo

//we can still access the &apos;anon&apos; class directly in the global scope! 
$another=get_class($test); //get the auto assigned name
echo $another::$str;    //outputs foo
?>
```


The above is functionally the same as doing this....


```
<?php
class I_named_this_one{
    public static $str="foo";
}
function return_not_anon(){
    return &apos;I_named_this_one&apos;;
}
$clzz=return_not_anon();//get class name
echo $clzz::$str;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.anonymous.php)

**[To root](/README.md)**