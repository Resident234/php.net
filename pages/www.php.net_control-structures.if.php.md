# if



easy way to execute conditional html / javascript / css / other language code with php if else:<br><br>

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

re: #80305<br><br>Again useful for newbies:<br><br>if you need to compare a variable with a value, instead of doing<br><br>

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
<br><br>and PHP will report an error.  

#

Any variables defined inside the if block will be available outside the block. Remember that the if doesn&apos;t have its own scope.<br><br>

```
<?php
$bool = true;
if ($bool) {
    $hi = 'Hello to all people!';
}
echo $hi;
?>
```


It will print 'Hello to all people!'

On the other hand, this will have no output:



```
<?php
if (false) {
    $hi = 'Hello to all people!';
}
echo $hi;
?>
```
  

#

An other way for controls is the ternary operator (see Comparison Operators) that can be used as follows:<br><br>

```
<?php
$v = 1;

$r = (1 == $v) ? 'Yes' : 'No'; // $r is set to 'Yes'
$r = (3 == $v) ? 'Yes' : 'No'; // $r is set to 'No'

echo (1 == $v) ? 'Yes' : 'No'; // 'Yes' will be printed

// and since PHP 5.3
$v = 'My Value';
$r = ($v) ?: 'No Value'; // $r is set to 'My Value' because $v is evaluated to TRUE

$v = '';
echo ($v) ?: 'No Value'; // 'No Value' will be printed because $v is evaluated to FALSE
?>
```
<br><br>Parentheses can be left out in all examples above.  

#

You can have &apos;nested&apos; if statements withing a single if statement, using additional parenthesis.<br>For example, instead of having:<br><br>

```
<?php
if( $a == 1 || $a == 2 ) {
    if( $b == 3 || $b == 4 ) {
        if( $c == 5 || $ d == 6 ) {
             //Do something here.
        }
    }
}
?>
```


You could just simply do this:



```
<?php
if( ($a==1 || $a==2) &amp;&amp; ($b==3 || $b==4) &amp;&amp; ($c==5 || $c==6) ) {
    //do that something here.
}
?>
```
<br><br>Hope this helps!  

#

In addition to the traditional syntax for if (condition) action;<br>I am fond of the ternary operator that does the same thing, but with fewer words and code to type:<br><br>(condition ? action_if_true: action_if_false;)<br><br>example<br><br>(x &gt; y? &apos;Passed the test&apos; : &apos;Failed the test&apos;)  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.if.php)

**[To root](/README.md)**