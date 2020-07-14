# Expressions



Manual defines "expression is anything that has value", Therefore, parser will give error for following code.<br><br>

```
<?php
($val) ? echo('true') : echo('false');
Note: "? : " operator has this syntax  "expr ? expr : expr;"
?>
```


since echo does not have(return) value and ?: expects expression(value).

However, if function/language constructs that have/return value, such as include(), parser compiles code.

Note: User defined functions always have/return value without explicit return statement (returns NULL if there is no return statement). Therefore, user defined functions are always valid expressions. 
[It may be useful to have VOID as new type to prevent programmer to use function as RVALUE by mistake]

For example,



```
<?php
($val) ? include('true.inc') : include('false.inc');
?>
```
<br><br>is valid, since "include" returns value.<br><br>The fact "echo" does not return value(="echo" is not a expression), is less obvious to me. <br><br>Print() and Echo() is NOT identical since print() has/returns value and can be a valid expression.  

#

Note that even though PHP borrows large portions of its syntax from C, the &apos;,&apos; is treated quite differently. It&apos;s not possible to create combined expressions in PHP using the comma-operator that C has, except in for() loops.<br><br>Example (parse error):<br><br>

```
<?php

$a = 2, $b = 4;

echo $a."\n";
echo $b."\n";

?>
```


Example (works):


```
<?php

for ($a = 2, $b = 4; $a &lt; 3; $a++)
{
  echo $a."\n";
  echo $b."\n";
}

?>
```
<br><br>This is because PHP doesn&apos;t actually have a proper comma-operator, it&apos;s only supported as syntactic sugar in for() loop headers. In C, it would have been perfectly legitimate to have this:<br><br>int f()<br>{<br>  int a, b;<br>  a = 2, b = 4;<br><br>  return a;<br>}<br><br>or even this:<br><br>int g()<br>{<br>  int a, b;<br>  a = (2, b = 4);<br><br>  return a;<br>}<br><br>In f(), a would have been set to 2, and b would have been set to 4.<br>In g(), (2, b = 4) would be a single expression which evaluates to 4, so both a and b would have been set to 4.  

#

Note that there is a difference between a function and a function call, and both<br>are expressions. PHP has two kinds of function, "named functions" and "anonymous<br>functions". Here&apos;s an example with both:<br><br>

```
<?php
// A named function. Its name is "double".
function double($x) {
  return 2 * $x;
}

// An anonymous function. It has no name, in the same way that the string
// "hello" has no name. Since it is an expression, we can give it a temporary
// name by assigning it to the variable $triple.
$triple = function($x) {
  return 3 * $x;
};
?>
```


We can "call" (or "run") both kinds of function. A "function call" is an
expression with the value of whatever the function returns. For example:



```
<?php
// The easiest way to run a function is to put () after its name, containing its
// arguments (if any)
$my_numbers = array(double(5), $triple(5));
?>
```


$my_numbers is now an array containing 10 and 15, which are the return values of
double and $triple when applied to the number 5.

Importantly, if we *don't* call a function, ie. we don't put () after its name,
then we still get expressions. For example:



```
<?php
$my_functions = array('double', $triple);
?>
```


$my_functions is now an array containing these two functions. Notice that named
functions are more awkward than anonymous functions. PHP treats them differently
because it didn't use to have anonymous functions, and the way named functions
were implemented didn't work for anonymous functions when they were eventually
added.

This means that instead of using a named function literally, like we can with
anonymous functions, we have to use a string containing its name instead. PHP
makes sure that these strings will be treated as functions when it's
appropriate. For example:



```
<?php
$temp      = 'double';
$my_number = $temp(5);
?>
```


$my_number will be 10, since PHP has spotted that we're treating a string as if
it were a function, so it has looked up that named function for us.

Unfortunately PHP's parser is very quirky; rather than looking for generic
patterns like "x(y)" and seeing if "x" is a function, it has lots of
special-cases like "$x(y)". This makes code like "'double'(5)" invalid, so we
have to do tricks like using temporary variables. There is another way around
this restriction though, and that is to pass our functions to the
"call_user_func" or "call_user_func_array" functions when we want to call them.
For example:



```
<?php
$my_numbers = array(call_user_func('double', 5), call_user_func($triple, 5));
?>
```


$my_numbers contains 10 and 15 because "call_user_func" called our functions for
us. This is possible because the string 'double' and the anonymous function
$triple are expressions. Note that we can even use this technique to call an
anonymous function without ever giving it a name:



```
<?php
$my_number = call_user_func(function($x) { return 4 * $x; }, 5);
?>
```
<br><br>$my_number is now 20, since "call_user_func" called the anonymous function,<br>which quadruples its argument, with the value 5.<br><br>Passing functions around as expressions like this is very useful whenever we<br>need to use a &apos;callback&apos;. Great examples of this are array_map and array_reduce.  

#

[Official documentation page](https://www.php.net/manual/en/language.expressions.php)

**[To root](/README.md)**