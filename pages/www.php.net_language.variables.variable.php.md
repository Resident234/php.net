# Variable variables







```
<?php

&#xA0; //You can even add more Dollar Signs

&#xA0; $Bar = &quot;a&quot;;
&#xA0; $Foo = &quot;Bar&quot;;
&#xA0; $World = &quot;Foo&quot;;
&#xA0; $Hello = &quot;World&quot;;
&#xA0; $a = &quot;Hello&quot;;

&#xA0; $a; //Returns Hello
&#xA0; $$a; //Returns World
&#xA0; $$$a; //Returns Foo
&#xA0; $$$$a; //Returns Bar
&#xA0; $$$$$a; //Returns a

&#xA0; $$$$$$a; //Returns Hello
&#xA0; $$$$$$$a; //Returns World

&#xA0; //... and so on ...//

?>
```



  

#



It may be worth specifically noting, if variable names follow some kind of &quot;template,&quot; they can be referenced like this:



```
<?php
// Given these variables ...
$nameTypes&#xA0; &#xA0; = array(&quot;first&quot;, &quot;last&quot;, &quot;company&quot;);
$name_first&#xA0;&#xA0; = &quot;John&quot;;
$name_last&#xA0; &#xA0; = &quot;Doe&quot;;
$name_company = &quot;PHP.net&quot;;

// Then this loop is ...
foreach($nameTypes as $type)
&#xA0; print ${&quot;name_$type&quot;} . &quot;\n&quot;;

// ... equivalent to this print statement.
print &quot;$name_first\n$name_last\n$name_company\n&quot;;
?>
```


This is apparent from the notes others have left, but is not explicitly stated.

  

#





```
<?php
// $variable-name = &apos;parse error&apos;;
// You can&apos;t do that but you can do this:
$a = &apos;variable-name&apos;;
$$a = &apos;hello&apos;;
echo $variable-name . &apos; &apos; . $$a; // Gives&#xA0; &#xA0;&#xA0; 0 hello
?>
```


For a particular reason I had been using some variable names with hyphens for ages. There was no problem because they were only referenced via a variable variable. I only saw a parse error much later, when I tried to reference one directly. It took a while to realise that illegal hyphens were the cause because the parse error only occurs on assignment.

  

#



PHP actually supports invoking a new instance of a class using a variable class name since at least version 5.2



```
<?php
class Foo {
&#xA0;&#xA0; public function hello() {
&#xA0; &#xA0; &#xA0; echo &apos;Hello world!&apos;;
&#xA0;&#xA0; }
}
$my_foo = &apos;Foo&apos;;
$a = new $my_foo();
$a-&gt;hello(); //prints &apos;Hello world!&apos;
?>
```


Additionally, you can access static methods and properties using variable class names, but only since PHP 5.3



```
<?php
class Foo {
&#xA0;&#xA0; public static function hello() {
&#xA0; &#xA0; &#xA0; echo &apos;Hello world!&apos;;
&#xA0;&#xA0; }
}
$my_foo = &apos;Foo&apos;;
$my_foo::hello(); //prints &apos;Hello world!&apos;
?>
```



  

#



You may think of using variable variables to dynamically generate variables from an array, by doing something similar to: -



```
<?php
 foreach ($array as $key =&gt; $value) 
 {
&#xA0; $$key= $value;
 }

?>
```


This however would be reinventing the wheel when you can simply use: 



```
<?php
extract( $array, EXTR_OVERWRITE);
?>
```


Note that this will overwrite the contents of variables that already exist.

Extract has useful functionality to prevent this, or you may group the variables by using prefixes too, so you could use: -

EXTR_PREFIX_ALL



```
<?php
$array =array(&quot;one&quot; =&gt; &quot;First Value&quot;,
&quot;two&quot; =&gt; &quot;2nd Value&quot;,
&quot;three&quot; =&gt; &quot;8&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
extract( $array, EXTR_PREFIX_ALL, &quot;my_prefix_&quot;);
&#xA0;&#xA0; 
?>
```


This would create variables: -
$my_prefix_one 
$my_prefix_two
$my_prefix_three

containing: -
&quot;First Value&quot;, &quot;2nd Value&quot; and &quot;8&quot; respectively

  

#



If you want to use a variable value in part of the name of a variable variable (not the whole name itself), you can do like the following:



```
<?php
$price_for_monday = 10;
$price_for_tuesday = 20;
$price_for_wednesday = 30;

$today = &apos;tuesday&apos;;

$price_for_today = ${ &apos;price_for_&apos; . $today};
echo $price_for_today; // will return 20
?>
```



  

#



Another use for this feature in PHP is dynamic parsing..&#xA0; 



Due to the rather odd structure of an input string I am currently parsing, I must have a reference for each particular object instantiation in the order which they were created.&#xA0; In addition, because of the syntax of the input string, elements of the previous object creation are required for the current one.&#xA0; 



Normally, you won&apos;t need something this convolute.&#xA0; In this example, I needed to load an array with dynamically named objects - (yes, this has some basic Object Oriented programming, please bare with me..)





```
<?php

&#xA0;&#xA0; include(&quot;obj.class&quot;);



&#xA0;&#xA0; // this is only a skeletal example, of course.

&#xA0;&#xA0; $object_array = array();



&#xA0;&#xA0; // assume the $input array has tokens for parsing.

&#xA0;&#xA0; foreach ($input_array as $key=&gt;$value){

&#xA0; &#xA0; &#xA0; // test to ensure the $value is what we need.

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $obj = &quot;obj&quot;.$key;

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $$obj = new Obj($value, $other_var);

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; Array_Push($object_array, $$obj);

&#xA0; &#xA0; &#xA0; // etc..

&#xA0;&#xA0; }



?>
```




Now, we can use basic array manipulation to get these objects out in the particular order we need, and the objects no longer are dependant on the previous ones.



I haven&apos;t fully tested the implimentation of the objects.&#xA0; The&#xA0; scope of a variable-variable&apos;s object attributes (get all that?) is a little tough to crack.&#xA0; Regardless, this is another example of the manner in which the var-vars can be used with precision where tedious, extra hard-coding is the only alternative.



Then, we can easily pull everything back out again using a basic array function: foreach.





```
<?php

//...

&#xA0;&#xA0; foreach($array as $key=&gt;$object){



&#xA0; &#xA0; &#xA0; echo $key.&quot; -- &quot;.$object-&gt;print_fcn().&quot; &lt;br/&gt;\n&quot;;



&#xA0;&#xA0; } // end foreach&#xA0;&#xA0; 



?>
```




Through this, we can pull a dynamically named object out of the array it was stored in without actually knowing its name.

  

#



While not relevant in everyday PHP programming, it seems to be possible to insert whitespace and comments between the dollar signs of a variable variable.&#xA0; All three comment styles work. This information becomes relevant when writing a parser, tokenizer or something else that operates on PHP syntax.



```
<?php

&#xA0; &#xA0; $foo = &apos;bar&apos;;
&#xA0; &#xA0; $

&#xA0; &#xA0; /*
&#xA0; &#xA0; &#xA0; &#xA0; I am complete legal and will compile without notices or error as a variable variable.
&#xA0; &#xA0; */
&#xA0; &#xA0; &#xA0; &#xA0; $foo = &apos;magic&apos;;

&#xA0; &#xA0; echo $bar; // Outputs magic.

?>
```


Behaviour tested with PHP Version 5.6.19

  

#



Adding an element directly to an array using variables:



```
<?php
$tab = array(&quot;one&quot;, &quot;two&quot;, &quot;three&quot;) ;
$a = &quot;tab&quot; ;
$$a[] =&quot;four&quot; ; // &lt;==== fatal error
print_r($tab) ;
?>
```

will issue this error:

Fatal error: Cannot use [] for reading

This is not a bug, you need to use the {} syntax to remove the ambiguity.



```
<?php
$tab = array(&quot;one&quot;, &quot;two&quot;, &quot;three&quot;) ;
$a = &quot;tab&quot; ;
${$a}[] =&#xA0; &quot;four&quot; ; // &lt;==== this is the correct way to do it
print_r($tab) ;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.variable.php)

**[To root](/README.md)**