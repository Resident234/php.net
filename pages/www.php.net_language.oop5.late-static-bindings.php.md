# Late Static Bindings





Finally we can implement some ActiveRecord methods:





```
<?php



class Model

{

&#xA0; &#xA0; public static function find()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; echo static::$name;

&#xA0; &#xA0; }

}



class Product extends Model

{

&#xA0; &#xA0; protected static $name = &apos;Product&apos;;

}



Product::find();



?>
```




Output: &apos;Product&apos;

  

#



For abstract classes with static factory method, you can use the static keyword instead of self like the following:


```
<?php

abstract class A{
&#xA0; &#xA0; 
&#xA0; &#xA0; static function create(){

&#xA0; &#xA0; &#xA0; &#xA0; //return new self();&#xA0; //Fatal error: Cannot instantiate abstract class A

&#xA0; &#xA0; &#xA0; &#xA0; return new static(); //this is the correct way

&#xA0; &#xA0; }
&#xA0; &#xA0; 
}

class B extends A{
}

$obj=B::create();
var_dump($obj);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.late-static-bindings.php)

**[To root](/README.md)**