# Variable variables





```
<?php

  //You can even add more Dollar Signs

  $Bar = "a";
  $Foo = "Bar";
  $World = "Foo";
  $Hello = "World";
  $a = "Hello";

  $a; //Returns Hello
  $a; //Returns World
  $$a; //Returns Foo
  $$a; //Returns Bar
  $$$a; //Returns a

  $$$a; //Returns Hello
  $$$$a; //Returns World

  //... and so on ...//

?>
```
  

#

It may be worth specifically noting, if variable names follow some kind of "template," they can be referenced like this:<br><br>

```
<?php
// Given these variables ...
$nameTypes    = array("first", "last", "company");
$name_first   = "John";
$name_last    = "Doe";
$name_company = "PHP.net";

// Then this loop is ...
foreach($nameTypes as $type)
  print ${"name_$type"} . "\n";

// ... equivalent to this print statement.
print "$name_first\n$name_last\n$name_company\n";
?>
```
<br><br>This is apparent from the notes others have left, but is not explicitly stated.  

#



```
<?php
// $variable-name = &apos;parse error&apos;;
// You can&apos;t do that but you can do this:
$a = &apos;variable-name&apos;;
$a = &apos;hello&apos;;
echo $variable-name . &apos; &apos; . $a; // Gives     0 hello
?>
```
<br><br>For a particular reason I had been using some variable names with hyphens for ages. There was no problem because they were only referenced via a variable variable. I only saw a parse error much later, when I tried to reference one directly. It took a while to realise that illegal hyphens were the cause because the parse error only occurs on assignment.  

#

PHP actually supports invoking a new instance of a class using a variable class name since at least version 5.2<br><br>

```
<?php
class Foo {
   public function hello() {
      echo &apos;Hello world!&apos;;
   }
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
   public static function hello() {
      echo &apos;Hello world!&apos;;
   }
}
$my_foo = &apos;Foo&apos;;
$my_foo::hello(); //prints &apos;Hello world!&apos;
?>
```
  

#

You may think of using variable variables to dynamically generate variables from an array, by doing something similar to: -<br><br>

```
<?php
 foreach ($array as $key =&gt; $value) 
 {
  $key= $value;
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
$array =array("one" =&gt; "First Value",
"two" =&gt; "2nd Value",
"three" =&gt; "8"
                );
           
extract( $array, EXTR_PREFIX_ALL, "my_prefix_");
   
?>
```
<br><br>This would create variables: -<br>$my_prefix_one <br>$my_prefix_two<br>$my_prefix_three<br><br>containing: -<br>"First Value", "2nd Value" and "8" respectively  

#

If you want to use a variable value in part of the name of a variable variable (not the whole name itself), you can do like the following:<br><br>

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

Another use for this feature in PHP is dynamic parsing..  <br><br>Due to the rather odd structure of an input string I am currently parsing, I must have a reference for each particular object instantiation in the order which they were created.  In addition, because of the syntax of the input string, elements of the previous object creation are required for the current one.  <br><br>Normally, you won&apos;t need something this convolute.  In this example, I needed to load an array with dynamically named objects - (yes, this has some basic Object Oriented programming, please bare with me..)<br><br>

```
<?php
   include("obj.class");

   // this is only a skeletal example, of course.
   $object_array = array();

   // assume the $input array has tokens for parsing.
   foreach ($input_array as $key=&gt;$value){
      // test to ensure the $value is what we need.
         $obj = "obj".$key;
         $obj = new Obj($value, $other_var);
         Array_Push($object_array, $obj);
      // etc..
   }

?>
```


Now, we can use basic array manipulation to get these objects out in the particular order we need, and the objects no longer are dependant on the previous ones.

I haven&apos;t fully tested the implimentation of the objects.  The  scope of a variable-variable&apos;s object attributes (get all that?) is a little tough to crack.  Regardless, this is another example of the manner in which the var-vars can be used with precision where tedious, extra hard-coding is the only alternative.

Then, we can easily pull everything back out again using a basic array function: foreach.



```
<?php
//...
   foreach($array as $key=&gt;$object){

      echo $key." -- ".$object-&gt;print_fcn()." &lt;br/&gt;\n";

   } // end foreach   

?>
```
<br><br>Through this, we can pull a dynamically named object out of the array it was stored in without actually knowing its name.  

#

While not relevant in everyday PHP programming, it seems to be possible to insert whitespace and comments between the dollar signs of a variable variable.  All three comment styles work. This information becomes relevant when writing a parser, tokenizer or something else that operates on PHP syntax.<br><br>

```
<?php

    $foo = &apos;bar&apos;;
    $

    /*
        I am complete legal and will compile without notices or error as a variable variable.
    */
        $foo = &apos;magic&apos;;

    echo $bar; // Outputs magic.

?>
```
<br><br>Behaviour tested with PHP Version 5.6.19  

#

Adding an element directly to an array using variables:<br><br>

```
<?php
$tab = array("one", "two", "three") ;
$a = "tab" ;
$a[] ="four" ; // &lt;==== fatal error
print_r($tab) ;
?>
```

will issue this error:

Fatal error: Cannot use [] for reading

This is not a bug, you need to use the {} syntax to remove the ambiguity.



```
<?php
$tab = array("one", "two", "three") ;
$a = "tab" ;
${$a}[] =  "four" ; // &lt;==== this is the correct way to do it
print_r($tab) ;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.variable.php)

**[To root](/README.md)**