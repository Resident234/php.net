# if





easy way to execute conditional html / javascript / css / other language code with php if else:



```
<?php if (condition): ?>
```


html code to run if condition is true



```
<?php else: ?>
```


html code to run if condition is false



```
<?php endif ?>
```



  

#



re: #80305

Again useful for newbies:

if you need to compare a variable with a value, instead of doing



```
<?php
if ($foo == 3) bar();
?>
```


do



```
<?php
if (3 == $foo) bar();
?>
```


this way, if you forget a =, it will become



```
<?php
if (3 = $foo) bar();
?>
```


and PHP will report an error.

  

#



Any variables defined inside the if block will be available outside the block. Remember that the if doesn&apos;t have its own scope.



```
<?php
$bool = true;
if ($bool) {
&#xA0; &#xA0; $hi = &apos;Hello to all people!&apos;;
}
echo $hi;
?>
```


It will print &apos;Hello to all people!&apos;

On the other hand, this will have no output:



```
<?php
if (false) {
&#xA0; &#xA0; $hi = &apos;Hello to all people!&apos;;
}
echo $hi;
?>
```



  

#



An other way for controls is the ternary operator (see Comparison Operators) that can be used as follows:



```
<?php
$v = 1;

$r = (1 == $v) ? &apos;Yes&apos; : &apos;No&apos;; // $r is set to &apos;Yes&apos;
$r = (3 == $v) ? &apos;Yes&apos; : &apos;No&apos;; // $r is set to &apos;No&apos;

echo (1 == $v) ? &apos;Yes&apos; : &apos;No&apos;; // &apos;Yes&apos; will be printed

// and since PHP 5.3
$v = &apos;My Value&apos;;
$r = ($v) ?: &apos;No Value&apos;; // $r is set to &apos;My Value&apos; because $v is evaluated to TRUE

$v = &apos;&apos;;
echo ($v) ?: &apos;No Value&apos;; // &apos;No Value&apos; will be printed because $v is evaluated to FALSE
?>
```


Parentheses can be left out in all examples above.

  

#



You can have &apos;nested&apos; if statements withing a single if statement, using additional parenthesis.

For example, instead of having:





```
<?php

if( $a == 1 || $a == 2 ) {

&#xA0; &#xA0; if( $b == 3 || $b == 4 ) {

&#xA0; &#xA0; &#xA0; &#xA0; if( $c == 5 || $ d == 6 ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //Do something here.

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}

?>
```




You could just simply do this:





```
<?php

if( ($a==1 || $a==2) &amp;&amp; ($b==3 || $b==4) &amp;&amp; ($c==5 || $c==6) ) {

&#xA0; &#xA0; //do that something here.

}

?>
```




Hope this helps!

  

#



In addition to the traditional syntax for if (condition) action;
I am fond of the ternary operator that does the same thing, but with fewer words and code to type:

(condition ? action_if_true: action_if_false;)

example

(x &gt; y? &apos;Passed the test&apos; : &apos;Failed the test&apos;)

  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.if.php)

**[To root](/README.md)**