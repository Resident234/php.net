# Late Static Bindings



Finally we can implement some ActiveRecord methods:<br><br>

```
<?php

class Model
{
    public static function find()
    {
        echo static::$name;
    }
}

class Product extends Model
{
    protected static $name = 'Product';
}

Product::find();

?>
```
<br><br>Output: &apos;Product&apos;  

#

For abstract classes with static factory method, you can use the static keyword instead of self like the following:<br>

```
<?php

abstract class A{
    
    static function create(){

        //return new self();  //Fatal error: Cannot instantiate abstract class A

        return new static(); //this is the correct way

    }
    
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