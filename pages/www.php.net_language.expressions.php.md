# Expressions





Manual defines &quot;expression is anything that has value&quot;, Therefore, parser will give error for following code.





```
<?php

($val) ? echo(&apos;true&apos;) : echo(&apos;false&apos;);

Note: &quot;? : &quot; operator has this syntax&#xA0; &quot;expr ? expr : expr;&quot;

?>
```




since echo does not have(return) value and ?: expects expression(value).



However, if function/language constructs that have/return value, such as include(), parser compiles code.



Note: User defined functions always have/return value without explicit return statement (returns NULL if there is no return statement). Therefore, user defined functions are always valid expressions. 

[It may be useful to have VOID as new type to prevent programmer to use function as RVALUE by mistake]



For example,





```
<?php

($val) ? include(&apos;true.inc&apos;) : include(&apos;false.inc&apos;);

?>
```




is valid, since &quot;include&quot; returns value.



The fact &quot;echo&quot; does not return value(=&quot;echo&quot; is not a expression), is less obvious to me. 



Print() and Echo() is NOT identical since print() has/returns value and can be a valid expression.

  

#



Note that even though PHP borrows large portions of its syntax from C, the &apos;,&apos; is treated quite differently. It&apos;s not possible to create combined expressions in PHP using the comma-operator that C has, except in for() loops.

Example (parse error):



```
<?php

$a = 2, $b = 4;

echo $a.&quot;\n&quot;;
echo $b.&quot;\n&quot;;

?>
```


Example (works):


```
<?php

for ($a = 2, $b = 4; $a &lt; 3; $a++)
{
&#xA0; echo $a.&quot;\n&quot;;
&#xA0; echo $b.&quot;\n&quot;;
}

?>
```


This is because PHP doesn&apos;t actually have a proper comma-operator, it&apos;s only supported as syntactic sugar in for() loop headers. In C, it would have been perfectly legitimate to have this:

int f()
{
&#xA0; int a, b;
&#xA0; a = 2, b = 4;

&#xA0; return a;
}

or even this:

int g()
{
&#xA0; int a, b;
&#xA0; a = (2, b = 4);

&#xA0; return a;
}

In f(), a would have been set to 2, and b would have been set to 4.
In g(), (2, b = 4) would be a single expression which evaluates to 4, so both a and b would have been set to 4.

  

#



Note that there is a difference between a function and a function call, and both
are expressions. PHP has two kinds of function, &quot;named functions&quot; and &quot;anonymous
functions&quot;. Here&apos;s an example with both:



```
<?php
// A named function. Its name is &quot;double&quot;.
function double($x) {
&#xA0; return 2 * $x;
}

// An anonymous function. It has no name, in the same way that the string
// &quot;hello&quot; has no name. Since it is an expression, we can give it a temporary
// name by assigning it to the variable $triple.
$triple = function($x) {
&#xA0; return 3 * $x;
};
?>
```


We can &quot;call&quot; (or &quot;run&quot;) both kinds of function. A &quot;function call&quot; is an
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

Importantly, if we *don&apos;t* call a function, ie. we don&apos;t put () after its name,
then we still get expressions. For example:



```
<?php
$my_functions = array(&apos;double&apos;, $triple);
?>
```


$my_functions is now an array containing these two functions. Notice that named
functions are more awkward than anonymous functions. PHP treats them differently
because it didn&apos;t use to have anonymous functions, and the way named functions
were implemented didn&apos;t work for anonymous functions when they were eventually
added.

This means that instead of using a named function literally, like we can with
anonymous functions, we have to use a string containing its name instead. PHP
makes sure that these strings will be treated as functions when it&apos;s
appropriate. For example:



```
<?php
$temp&#xA0; &#xA0; &#xA0; = &apos;double&apos;;
$my_number = $temp(5);
?>
```


$my_number will be 10, since PHP has spotted that we&apos;re treating a string as if
it were a function, so it has looked up that named function for us.

Unfortunately PHP&apos;s parser is very quirky; rather than looking for generic
patterns like &quot;x(y)&quot; and seeing if &quot;x&quot; is a function, it has lots of
special-cases like &quot;$x(y)&quot;. This makes code like &quot;&apos;double&apos;(5)&quot; invalid, so we
have to do tricks like using temporary variables. There is another way around
this restriction though, and that is to pass our functions to the
&quot;call_user_func&quot; or &quot;call_user_func_array&quot; functions when we want to call them.
For example:



```
<?php
$my_numbers = array(call_user_func(&apos;double&apos;, 5), call_user_func($triple, 5));
?>
```


$my_numbers contains 10 and 15 because &quot;call_user_func&quot; called our functions for
us. This is possible because the string &apos;double&apos; and the anonymous function
$triple are expressions. Note that we can even use this technique to call an
anonymous function without ever giving it a name:



```
<?php
$my_number = call_user_func(function($x) { return 4 * $x; }, 5);
?>
```


$my_number is now 20, since &quot;call_user_func&quot; called the anonymous function,
which quadruples its argument, with the value 5.

Passing functions around as expressions like this is very useful whenever we
need to use a &apos;callback&apos;. Great examples of this are array_map and array_reduce.

  

#

[Official documentation page](https://www.php.net/manual/en/language.expressions.php)

**[To root](/README.md)**