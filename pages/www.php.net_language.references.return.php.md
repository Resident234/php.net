# Returning References



a little addition to the example of pixel at minikomp dot com here below<br>

```
<?php

    function &amp;func(){
        static $static = 0;
        $static++;
        return $static;
    }

    $var1 =&amp; func();
    echo "var1:", $var1; // 1
    func();
    func();
    echo "var1:", $var1; // 3
    $var2 = func(); // assignment without the &amp;
    echo "var2:", $var2; // 4
    func();
    func();
    echo "var1:", $var1; // 6
    echo "var2:", $var2; // still 4

?>
```
  

#

Sometimes, you would like to return NULL with a function returning reference, to indicate the end of chain of elements. However this generates E_NOTICE. Here is little tip, how to prevent that:<br><br>

```
<?php
class Foo {
   const $nullGuard = NULL;
   // ... some declarations and definitions
   public function &amp;next() {
      // ...
      if (!$end) return $bar;
      else return $this->nullGuard;
   }
}
?>
```


by doing this you can do smth like this without notices:



```
<?php
$f = new Foo();
// ...
while (($item = $f->next()) != NULL) {
// ...
}
?>
```
<br><br>you may also use global variable:<br>global $nullGuard;<br>return $nullGuard;  

#

I haven&apos;t seen anyone note method chaining in PHP5.  When an object is returned by a method in PHP5 it is returned by default as a reference, and the new Zend Engine 2 allows you to chain method calls from those returned objects.  For example consider this code:<br><br>

```
<?php

class Foo {

    protected $bar;

    public function __construct() {
        $this->bar = new Bar();

        print "Foo\n";
    }    
    
    public function getBar() {
        return $this->bar;
    }
}

class Bar {

    public function __construct() {
        print "Bar\n";
    }
    
    public function helloWorld() {
        print "Hello World\n";
    }
}

function test() {
    return new Foo();
}

test()->getBar()->helloWorld();

?>
```
<br><br>Notice how we called test() which was not on an object, but returned an instance of Foo, followed by a method on Foo, getBar() which returned an instance of Bar and finally called one of its methods helloWorld().  Those familiar with other interpretive languages (Java to name one) will recognize this functionality.  For whatever reason this change doesn&apos;t seem to be documented very well, so hopefully someone will find this helpful.  

#

An example of returning references:<br><br>&lt;?<br><br>$var = 1;<br>$num = NULL;<br><br>function &amp;blah()<br>{<br>    $var =&amp; $GLOBALS["var"]; # the same as global $var;<br>    $var++;<br>    return $var;<br>}<br><br>$num = &amp;blah();<br><br>echo $num; # 2<br><br>blah();<br><br>echo $num; # 3<br><br>?>
```
<br><br>Note: if you take the &amp; off from the function, the second echo will be 2, because without &amp; the var $num contains its returning value and not its returning reference.  

#

[Official documentation page](https://www.php.net/manual/en/language.references.return.php)

**[To root](/README.md)**