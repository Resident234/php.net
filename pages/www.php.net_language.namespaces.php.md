# Namespaces



The keyword &apos;use&apos; has two different applications, but the reserved word table links to here.<br><br>It can apply to namespace constucts:<br><br>file1:<br>

```
<?php namespace foo;
  class Cat { 
    static function says() {echo 'meoow';}  } ?>
```


file2:


```
<?php namespace bar;
  class Dog {
    static function says() {echo 'ruff';}  } ?>
```


file3:


```
<?php namespace animate;
  class Animal {
    static function breathes() {echo 'air';}  } ?>
```


file4:


```
<?php namespace fub;
  include 'file1.php';
  include 'file2.php';
  include 'file3.php';
  use foo as feline;
  use bar as canine;
  use animate;
  echo \feline\Cat::says(), "<br />\n";
  echo \canine\Dog::says(), "<br />\n";
  echo \animate\Animal::breathes(), "<br />\n";  ?>
```


Note that 
felineCat::says()
should be
\feline\Cat::says()
(and similar for the others)
but this comment form deletes the backslash (why???) 

The 'use' keyword also applies to closure constructs:



```
<?php function getTotal($products_costs, $tax)
    {
        $total = 0.00;
        
        $callback =
            function ($pricePerItem) use ($tax, &amp;$total)
            {
                
                $total += $pricePerItem * ($tax + 1.0);
            };
        
        array_walk($products_costs, $callback);
        return round($total, 2);
    }
?>
```
  

#

Tested on PHP 7.0.5, Windows<br>The line "use animate;" equals the line "use animate as animate;"<br>but the "use other\animate;" equals "use other\animate as animate;"<br><br>file1:<br>

```
<?php namespace foo;
  class Cat { 
    static function says() {echo 'meoow';}  } ?>
```


file2:


```
<?php namespace bar;
  class Dog {
    static function says() {echo 'ruff';}  } ?>
```


file3:


```
<?php namespace other\animate;
  class Animal {
    static function breathes() {echo 'air';}  } ?>
```


file4:


```
<?php namespace fub;
  include 'file1.php';
  include 'file2.php';
  include 'file3.php';
  use foo as feline;
  use bar as canine;
  use other\animate;       //use other\animate as animate;
  echo feline\Cat::says(), "<br />\n";
  echo canine\Dog::says(), "<br />\n";
  echo \animate\Animal::breathes(), "<br />\n";  ?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.php)

**[To root](/README.md)**