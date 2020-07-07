# Namespaces





The keyword &apos;use&apos; has two different applications, but the reserved word table links to here.

It can apply to namespace constucts:

file1:


```
<?php namespace foo;
&#xA0; class Cat { 
&#xA0; &#xA0; static function says() {echo &apos;meoow&apos;;}&#xA0; } ?>
```


file2:


```
<?php namespace bar;
&#xA0; class Dog {
&#xA0; &#xA0; static function says() {echo &apos;ruff&apos;;}&#xA0; } ?>
```


file3:


```
<?php namespace animate;
&#xA0; class Animal {
&#xA0; &#xA0; static function breathes() {echo &apos;air&apos;;}&#xA0; } ?>
```


file4:


```
<?php namespace fub;
&#xA0; include &apos;file1.php&apos;;
&#xA0; include &apos;file2.php&apos;;
&#xA0; include &apos;file3.php&apos;;
&#xA0; use foo as feline;
&#xA0; use bar as canine;
&#xA0; use animate;
&#xA0; echo \feline\Cat::says(), &quot;&lt;br /&gt;\n&quot;;
&#xA0; echo \canine\Dog::says(), &quot;&lt;br /&gt;\n&quot;;
&#xA0; echo \animate\Animal::breathes(), &quot;&lt;br /&gt;\n&quot;;&#xA0; ?>
```


Note that 
felineCat::says()
should be
\feline\Cat::says()
(and similar for the others)
but this comment form deletes the backslash (why???) 

The &apos;use&apos; keyword also applies to closure constructs:



```
<?php function getTotal($products_costs, $tax)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $total = 0.00;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $callback =
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; function ($pricePerItem) use ($tax, &amp;$total)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $total += $pricePerItem * ($tax + 1.0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; };
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; array_walk($products_costs, $callback);
&#xA0; &#xA0; &#xA0; &#xA0; return round($total, 2);
&#xA0; &#xA0; }
?>
```



  

#



Tested on PHP 7.0.5, Windows
The line &quot;use animate;&quot; equals the line &quot;use animate as animate;&quot;
but the &quot;use other\animate;&quot; equals &quot;use other\animate as animate;&quot;

file1:


```
<?php namespace foo;
&#xA0; class Cat { 
&#xA0; &#xA0; static function says() {echo &apos;meoow&apos;;}&#xA0; } ?>
```


file2:


```
<?php namespace bar;
&#xA0; class Dog {
&#xA0; &#xA0; static function says() {echo &apos;ruff&apos;;}&#xA0; } ?>
```


file3:


```
<?php namespace other\animate;
&#xA0; class Animal {
&#xA0; &#xA0; static function breathes() {echo &apos;air&apos;;}&#xA0; } ?>
```


file4:


```
<?php namespace fub;
&#xA0; include &apos;file1.php&apos;;
&#xA0; include &apos;file2.php&apos;;
&#xA0; include &apos;file3.php&apos;;
&#xA0; use foo as feline;
&#xA0; use bar as canine;
&#xA0; use other\animate;&#xA0; &#xA0; &#xA0;&#xA0; //use other\animate as animate;
&#xA0; echo feline\Cat::says(), &quot;&lt;br /&gt;\n&quot;;
&#xA0; echo canine\Dog::says(), &quot;&lt;br /&gt;\n&quot;;
&#xA0; echo \animate\Animal::breathes(), &quot;&lt;br /&gt;\n&quot;;&#xA0; ?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.php)

**[To root](/README.md)**