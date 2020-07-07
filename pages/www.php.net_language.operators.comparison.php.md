# Comparison of floating point numbers





I couldn&apos;t find much info on stacking the new ternary operator, so I ran some tests:



```
<?php
echo 0 ?: 1 ?: 2 ?: 3; //1
echo 1 ?: 0 ?: 3 ?: 2; //1
echo 2 ?: 1 ?: 0 ?: 3; //2
echo 3 ?: 2 ?: 1 ?: 0; //3

echo 0 ?: 1 ?: 2 ?: 3; //1
echo 0 ?: 0 ?: 2 ?: 3; //2
echo 0 ?: 0 ?: 0 ?: 3; //3
?>
```


It works just as expected, returning the first non-false value within a group of expressions.

  

#



[Editor&apos;s note: consider using ===]



I discover after 10 years of PHP development something awfull : even if you make a string comparison (both are strings), strings are tested like integers and leading &quot;space&quot; character (even \n, \r, \t) is ignored ....



I spent hours because of leading \n in a string ... it hurts my developper sensibility to see two strings beeing compared like integers and not like strings ... I use strcmp now for string comparison ... so stupid ...



Test code :



```
<?php



test(&quot;1234&quot;, &quot;1234&quot;);

test(&quot;1234&quot;, &quot; 1234&quot;);

test(&quot;1234&quot;, &quot;\n1234&quot;);

test(&quot;1234&quot;, &quot;1234 &quot;);

test(&quot;1234&quot;, &quot;1234\n&quot;);



function test($v1, $v2) {

&#xA0; &#xA0; echo &quot;&lt;h1&gt;[&quot;.show_cr($v1).&quot;] vs [&quot;.show_cr($v2).&quot;]&lt;/h1&gt;&quot;;

&#xA0; &#xA0; echo my_var_dump($v1).&quot;&lt;br /&gt;&quot;;

&#xA0; &#xA0; echo my_var_dump($v2).&quot;&lt;br /&gt;&quot;;

&#xA0; &#xA0; if($v1 == $v2) {

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;EQUAL !&quot;;

&#xA0; &#xA0; }

&#xA0; &#xA0; else {

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;DIFFERENT !&quot;;

&#xA0; &#xA0; }

}



function show_cr($var) {

&#xA0; &#xA0; return str_replace(&quot;\n&quot;, &quot;\\n&quot;, $var);

}



function my_var_dump($var) {

&#xA0; &#xA0; ob_start();

&#xA0; &#xA0; var_dump($var);

&#xA0; &#xA0; $dump = show_cr(trim(ob_get_contents()));

&#xA0; &#xA0; ob_end_clean();

&#xA0; &#xA0; return $dump;

}



?>
```




Displays this -&gt;



[1234] vs [1234]

string(4) &quot;1234&quot;

string(4) &quot;1234&quot;

EQUAL !



[1234] vs [ 1234]

string(4) &quot;1234&quot;

string(5) &quot; 1234&quot;

EQUAL !



[1234] vs [\n1234]

string(4) &quot;1234&quot;

string(5) &quot;\n1234&quot;

EQUAL !



[1234] vs [1234 ]

string(4) &quot;1234&quot;

string(5) &quot;1234 &quot;

DIFFERENT !



[1234] vs [1234\n]

string(4) &quot;1234&quot;

string(5) &quot;1234\n&quot;

DIFFERENT !

  

#



note: the behavior below is documented in the appendix K about type comparisons, but since it is somewhat buried i thought i should raise it here for people since it threw me for a loop until i figured it out completely.

just to clarify a tricky point about the == comparison operator when dealing with strings and numbers:

(&apos;some string&apos; == 0) returns TRUE

however, (&apos;123&apos; == 0) returns FALSE

also note that ((int) &apos;some string&apos;) returns 0

and ((int) &apos;123&apos;) returns 123

the behavior makes senes but you must be careful when comparing strings to numbers, e.g. when you&apos;re comparing a request variable which you expect to be numeric. its easy to fall into the trap of:

if ($_GET[&apos;myvar&apos;]==0) dosomething();

as this will dosomething() even when $_GET[&apos;myvar&apos;] is &apos;some string&apos; and clearly not the value 0

i was getting lazy with my types since php vars are so flexible, so be warned to pay attention to the details...

  

#



I was interested about the following two uses of the ternary operator (PHP &gt;= 5.3) for using a &quot;default&quot; value if a variable is not set or evaluates to false:



```
<?php
(isset($some_variable) &amp;&amp; $some_variable) ? $some_variable : &apos;default_value&apos;;

$some_variable ?: &apos;default_value&apos;;
?>
```


The second is more readable, but will throw an ERR_NOTICE is $some_variable is not set. Of course, this could be overcome by suppressing the notice using the @ operator.

Performance-wise, though, comparing 1 million iterations of the three statements

&#xA0; (isset($foo) &amp;&amp; $foo) ? $foo : &apos;&apos;
&#xA0; ($foo) ?: &apos;&apos;
&#xA0; (@$foo) ?: &apos;&apos;

results in the following:

&#xA0; $foo is NOT SET.
&#xA0; &#xA0; [isset] 0.18222403526306
&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.57496404647827
&#xA0; &#xA0; [@ ?:]&#xA0; 0.64780592918396
&#xA0; $foo is NULL.
&#xA0; &#xA0; [isset] 0.17995285987854
&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.15304207801819
&#xA0; &#xA0; [@ ?:]&#xA0; 0.20394206047058
&#xA0; $foo is FALSE.
&#xA0; &#xA0; [isset] 0.19388508796692
&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.15359902381897
&#xA0; &#xA0; [@ ?:]&#xA0; 0.20741701126099
&#xA0; $foo is TRUE.
&#xA0; &#xA0; [isset] 0.17265486717224
&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.11773896217346
&#xA0; &#xA0; [@ ?:]&#xA0; 0.16193103790283

In other words, using the long-form ternary operator with isset($some_variable) is preferable overall if $some_variable may not be set.

(error_reporting was set to zero for the benchmark, to avoid printing a million notices...)

  

#



Be careful when using the ternary operator!

The following will not evaluate to the expected result:



```
<?php
echo &quot;a string that has a &quot; . (true) ? &apos;true&apos; : &apos;false&apos; . &quot; condition in. &quot;;
?>
```


Will print true.

Instead, use this:



```
<?php
echo &quot;a string that has a &quot; . ((true) ? &apos;true&apos; : &apos;false&apos;) . &quot; condition in. &quot;;
?>
```


This will evaluate to the expected result: &quot;a string that has a true condition in. &quot;

I hope this helps.

  

#



Be careful with the &quot;==&quot; operator when both operands are strings:


```
<?php
var_dump(&apos;123&apos; == &apos;&#xA0; &#xA0; &#xA0;&#xA0; 123&apos;); // true
var_dump(&apos;1e3&apos; == &apos;1000&apos;); // true
var_dump(&apos;+74951112233&apos; == &apos;74951112233&apos;); // true
var_dump(&apos;00000020&apos; == &apos;0000000000000000020&apos;); // true
var_dump(&apos;0X1D&apos; == &apos;29E0&apos;); // true
var_dump(&apos;0xafebac&apos; == &apos;11529132&apos;); // true
var_dump(&apos;0xafebac&apos; == &apos;0XAFEBAC&apos;); // true
var_dump(&apos;0xeb&apos; == &apos;+235e-0&apos;); // true
var_dump(&apos;0.235&apos; == &apos;+.235&apos;); // true
var_dump(&apos;0.2e-10&apos; == &apos;2.0E-11&apos;); // true
var_dump(&apos;61529519452809720693702583126814&apos; == &apos;61529519452809720000000000000000&apos;); // true in php &lt; 5.4.4


  

#



The following contrasts the trinary operator associativity in PHP and Java.&#xA0; The first test would work as expected in Java (evaluates left-to-right, associates right-to-left, like if stmnt), the second in PHP (evaluates and associates left-to-right)



```
<?php

echo &quot;\n\n######----------- trinary operator associativity\n\n&quot;;

function trinaryTest($foo){

&#xA0; &#xA0; $bar&#xA0; &#xA0; = $foo &gt; 20
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ? &quot;greater than 20&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : $foo &gt; 10
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ? &quot;greater than 10&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : $foo &gt; 5
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ? &quot;greater than 5&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : &quot;not worthy of consideration&quot;;&#xA0; &#xA0; 
&#xA0; &#xA0; echo $foo.&quot; =&gt;&#xA0; &quot;.$bar.&quot;\n&quot;;
}

echo &quot;----trinaryTest\n\n&quot;;
trinaryTest(21);
trinaryTest(11);
trinaryTest(6);
trinaryTest(4);

function trinaryTestParens($foo){
&#xA0; &#xA0; 
&#xA0; &#xA0; $bar&#xA0; &#xA0; = $foo &gt; 20
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ? &quot;greater than 20&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : ($foo &gt; 10
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ? &quot;greater than 10&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : ($foo &gt; 5
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ? &quot;greater than 5&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : &quot;not worthy of consideration&quot;));&#xA0; &#xA0; 
&#xA0; &#xA0; echo $foo.&quot; =&gt;&#xA0; &quot;.$bar.&quot;\n&quot;;
}

echo &quot;----trinaryTestParens\n\n&quot;;
trinaryTestParens(21);
trinaryTestParens(11);
trinaryTest(6);
trinaryTestParens(4);

?>
```


Output:

######----------- trinary operator associativity 

----trinaryTest

21 =&gt;&#xA0; greater than 5
11 =&gt;&#xA0; greater than 5
6 =&gt;&#xA0; greater than 5
4 =&gt;&#xA0; not worthy of consideration

----trinaryTestParens

21 =&gt;&#xA0; greater than 20
11 =&gt;&#xA0; greater than 10
6 =&gt;&#xA0; greater than 5
4 =&gt;&#xA0; not worthy of consideration

  

#



if you want to use the ?: operator, you should be careful with the precedence.



Here&apos;s an example of the priority of operators:





```
<?php

echo &apos;Hello, &apos; . isset($i) ? &apos;my friend: &apos; . $username . &apos;, how are you doing?&apos; : &apos;my guest, &apos; . $guestusername . &apos;, please register&apos;;

?>
```




This make &quot;&apos;Hello, &apos; . isset($i)&quot; the sentence to evaluate. So, if you think to mix more sentences with the ?: operator, please use always parentheses to force the proper evaluation of the sentence.





```
<?php

echo &apos;Hello, &apos; . (isset($i) ? &apos;my friend: &apos; . $username . &apos;, how are you doing?&apos; : &apos;my guest, &apos; . $guestusername . &apos;, please register&apos;);

?>
```




for general rule, if you mix ?: with other sentences, always close it with parentheses.

  

#



For converted Perl programmers: use strict comparison operators (===, !==) in place of string comparison operators (eq, ne). Don&apos;t use the simple equality operators (==, !=), because ($a == $b) will return TRUE in many situations where ($a eq $b) would return FALSE.



For instance...

&quot;mary&quot; == &quot;fred&quot; is FALSE, but

&quot;+010&quot; == &quot;10.0&quot; is TRUE (!)



In the following examples, none of the strings being compared are identical, but because PHP *can* evaluate them as numbers, it does so, and therefore finds them equal...





```
<?php



echo (&quot;007&quot; == &quot;7&quot; ? &quot;EQUAL&quot; : &quot;not equal&quot;);

// Prints: EQUAL



// Surrounding the strings with single quotes (&apos;) instead of double

// quotes (&quot;) to ensure the contents aren&apos;t evaluated, and forcing

// string types has no effect.

echo ( (string)&apos;0001&apos; == (string)&apos;+1.&apos; ? &quot;EQUAL&quot; : &quot;not equal&quot;);

// Prints: EQUAL



// Including non-digit characters (like leading spaces, &quot;e&quot;, the plus

// or minus sign, period, ...) can still result in this behavior, if

// a string happens to be valid scientific notation.

echo (&apos;&#xA0; 131e-2&apos; == &apos;001.3100&apos; ? &quot;EQUAL&quot; : &quot;not equal&quot;);

// Prints: EQUAL



?>
```




If you&apos;re comparing passwords (or anything else for which &quot;near&quot; precision isn&apos;t good enough) this confusion could be detrimental. Stick with strict comparisons...





```
<?php



// Same examples as above, using === instead of ==



echo (&quot;007&quot; === &quot;7&quot; ? &quot;EQUAL&quot; : &quot;not equal&quot;);

// Prints: not equal



echo ( (string)&apos;0001&apos; === (string)&apos;+1.&apos; ? &quot;EQUAL&quot; : &quot;not equal&quot;);

// Prints: not equal



echo (&apos;&#xA0; 131e-2&apos; === &apos;001.3100&apos; ? &quot;EQUAL&quot; : &quot;not equal&quot;);

// Prints: not equal



?>
```



  

#



A quick way to do mysql bit comparison in php is to use the special character it stores . e.g


```
<?php
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($AvailableRequests[&apos;OngoingService&apos;] == &apos;&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&lt;td&gt;Yes&lt;/td&gt;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&lt;td&gt;No&lt;/td&gt;&apos;;

?>
```



  

#



Note: according to the spec, PHP&apos;s comparison operators are not transitive.&#xA0; For example, the following are all true in PHP5:



&quot;11&quot; &lt; &quot;a&quot; &lt; 2 &lt; &quot;11&quot;



As a result, the outcome of sorting an array depends on the order the elements appear in the pre-sort array.&#xA0; The following code will dump out two arrays with *different* orderings:





```
<?php

$a = array(2,&#xA0; &#xA0; &quot;a&quot;,&#xA0; &quot;11&quot;, 2);

$b = array(2,&#xA0; &#xA0; &quot;11&quot;, &quot;a&quot;,&#xA0; 2);

sort($a);

var_dump($a);

sort($b);

var_dump($b);

?>
```




This is not a bug report -- given the spec on this documentation page, what PHP does is &quot;correct&quot;.&#xA0; But that may not be what was intended...

  

#



Beware of the consequences of comparing strings to numbers.&#xA0; You can disprove the laws of the universe.

echo (&apos;X&apos; == 0 &amp;&amp; &apos;X&apos; == true &amp;&amp; 0 == false) ? &apos;true == false&apos; : &apos;sanity prevails&apos;;

This will output &apos;true == false&apos;.&#xA0; This stems from the use of the UNIX function strtod() to convert strings to numbers before comparing.&#xA0; Since &apos;X&apos; or any other string without a number in it converts to 0 when compared to a number, 0 == 0 &amp;&amp; &apos;X&apos; == true &amp;&amp; 0 == false

  

#



You can&apos;t just compare two arrays with the === operator

like you would think to find out if they are equal or not.&#xA0; This is more complicated when you have multi-dimensional arrays.&#xA0; Here is a recursive comparison function.





```
<?php

/**

 * Compares two arrays to see if they contain the same values.&#xA0; Returns TRUE or FALSE.

 * usefull for determining if a record or block of data was modified (perhaps by user input)

 * prior to setting a &quot;date_last_updated&quot; or skipping updating the db in the case of no change.

 *

 * @param array $a1

 * @param array $a2

 * @return boolean

 */

function array_compare_recursive($a1, $a2)

{

&#xA0;&#xA0; if (!(is_array($a1) and (is_array($a2)))) { return FALSE;}&#xA0; &#xA0; 

&#xA0; &#xA0; 

&#xA0;&#xA0; if (!count($a1) == count($a2)) 

&#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0;&#xA0; return FALSE; // arrays don&apos;t have same number of entries

&#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; 

&#xA0;&#xA0; foreach ($a1 as $key =&gt; $val) 

&#xA0;&#xA0; {

&#xA0; &#xA0; &#xA0;&#xA0; if (!array_key_exists($key, $a2)) 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {return FALSE; // uncomparable array keys don&apos;t match

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } 

&#xA0; &#xA0; &#xA0;&#xA0; elseif (is_array($val) and is_array($a2[$key]))&#xA0; // if both entries are arrays then compare recursive 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {if (!array_compare_recursive($val,$a2[$key])) return FALSE;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } 

&#xA0; &#xA0; &#xA0;&#xA0; elseif (!($val === $a2[$key])) // compare entries must be of same type.

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {return FALSE;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }

&#xA0;&#xA0; }

&#xA0;&#xA0; return TRUE; // $a1 === $a2

}

?>
```



  

#



I think everybody should read carefully what &quot;jeronimo at DELETE_THIS dot transartmedia dot com&quot; wrote. It&apos;s a great pitfall even for seasoned programmers and should be looked upon with a great attention.
For example, comparing passwords with == may result in a very large security hole.

I would add some more to it:

The workaround is to use strcmp() or ===.

Note on ===:

While the php documentation says that, basically,
($a===$b)&#xA0; is the same as&#xA0; ($a==$b &amp;&amp; gettype($a) == gettype($b)),
this is not true.

The difference between == and === is that === never does any type conversion. So, while, according to documentation, (&quot;+0.1&quot; === &quot;.1&quot;) should return true (because both are strings and == returns true), === actually returns false (which is good).

  

#



beware of the fact, that there is no `&lt;==` nor `&gt;==` therefore `false &lt;= 0` will be `true`. php v. 5.4.27

  

#



When you want to know if two arrays contain the same values, regardless of the values&apos; order, you cannot use &quot;==&quot; or &quot;===&quot;.&#xA0; In other words:



```
<?php
(array(1,2) == array(2,1)) === false;
?>
```


To answer that question, use:



```
<?php
function array_equal($a, $b) {
&#xA0; &#xA0; return (is_array($a) &amp;&amp; is_array($b) &amp;&amp; array_diff($a, $b) === array_diff($b, $a));
}
?>
```


A related, but more strict problem, is if you need to ensure that two arrays contain the same key=&gt;value pairs, regardless of the order of the pairs.&#xA0; In that case, use:



```
<?php
function array_identical($a, $b) {
&#xA0; &#xA0; return (is_array($a) &amp;&amp; is_array($b) &amp;&amp; array_diff_assoc($a, $b) === array_diff_assoc($b, $a));
}
?>
```


Example:


```
<?php
$a = array (2, 1);
$b = array (1, 2);
// true === array_equal($a, $b);
// false === array_identical($a, $b);

$a = array (&apos;a&apos; =&gt; 2, &apos;b&apos; =&gt; 1);
$b = array (&apos;b&apos; =&gt; 1, &apos;a&apos; =&gt; 2);
// true === array_identical($a, $b)
// true === array_equal($a, $b)
?>
```


(See also the solution &quot;rshawiii at yahoo dot com&quot; posted)

  

#



In the table &quot;Comparison with Various Types&quot;, please move the last line about &quot;Object&quot; to be above the line about &quot;Array&quot;, since Object is considered to be greater than Array (tested on 5.3.3)

(Please remove my &quot;Anonymous&quot; post of the same content before. You could check IP to see that I forgot to type my name)

  

#



Note that typecasting will NOT prevent the default behavior for converting two numeric strings to numbers when comparing them.



e.g.:





```
<?php

if ((string) &apos;0123&apos; == (string) &apos;123&apos;)

&#xA0; &#xA0; print &apos;equals&apos;;

else

&#xA0; &#xA0; print &apos;doesn\&apos;t equal&apos;;

?>
```




Still prints &apos;equals&apos;



As far as I can tell the only way to avoid this is to use the identity comparison operators (=== and !==).

  

#



Note: The ternary shortcut currently seems to be of no use in dealing with unexisting keys in an array, as PHP will throw an error. Take the following example.



```
<?php
$_POST[&apos;Unexisting&apos;] = $_POST[&apos;Unexisting&apos;] ?: false;
?>
```


PHP will throw an error that the &quot;Unexisting&quot; key does not exist. The @ operator does not work here to suppress this error.

  

#



a function to help settings default values, it returns its own first non-empty argument :

make your own eor combos !



```
<?php

/*
 * Either Or
 *
 * usage:&#xA0; $foo = eor(test1(),test2(),&quot;default&quot;);
 * usage:&#xA0; $foo = eor($_GET[&apos;foo&apos;], foogen(), $foo, &quot;bar&quot;);
 */

function eor() {
&#xA0; &#xA0; $vars = func_get_args();
&#xA0; &#xA0;&#xA0; while (!empty($vars) &amp;&amp; empty($defval))&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $defval = array_shift($vars);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0;&#xA0; return $defval;
}

 

?>
```



  

#



If you need nested ifs on I var its important to group the if so it works.
Example:


```
<?php
//Dont Works
//Parse error: parse error, unexpected &apos;:&apos; 
 $var=&apos;&lt;option value=&quot;1&quot; &apos;.$status == &quot;1&quot; ? &apos;selected=&quot;selected&quot;&apos; :&apos;&apos;.&apos;&gt;Value 1&lt;/option&gt;&apos;;
 //Works:
 $var=&apos;&lt;option value=&quot;1&quot; &apos;.($status == &quot;1&quot; ? &apos;selected=&quot;selected&quot;&apos; :&apos;&apos;).&apos;&gt;Value 1&lt;/option&gt;&apos;;

echo $var;
?>
```



  

#



Note that the &quot;ternary operator&quot; is better described as the &quot;conditional operator&quot;. The former name merely notes that it has three arguments without saying anything about what it does. Needless to say, if PHP picked up any more ternary operators, this will be a problem.

&quot;Conditional Operator&quot; is actually descriptive of the semantics, and is the name historically given to it in, e.g., C.

  

#



I came across peculiar outputs while I was attempting to debug a script





```
<?php

# Setup platform (pre conditions somewhere in a loop)

$index=1;

$tally = array();



# May work with warnings that $tally[$index] is not initialized

# Notice: Undefined offset: 1 in D:\htdocs\colors\ColorCompare\i.php on line #__

# It is an old fashioned way.

# $tally[$index] = $tally[$index] + 1;



# Does not work: Loops to attempt to change $index and values are aways unaffected

$tally[$index] = isset($tally[$index])?$tally[$index]:0+1;

$tally[$index] = isset($tally[$index])?$tally[$index]:0+1;

$tally[$index] = isset($tally[$index])?$tally[$index]:0+1;

/*

# These three lines output:

Array

(

&#xA0; &#xA0; [1] =&gt; 1

)

*/



# Works: This is what I need/expect

# $tally[$index] = 1+(isset($tally[$index])?$tally[$index]:0);



print_r($tally);

?>
```




The second block obviously does not work what one expects.

Third part is good.

  

#



With Nested ternary Operators you have to set the logical&#xA0; parentheses to get the correct result.



```
<?php
$test=true;
$test2=true;

($test) ? &quot;TEST1 true&quot; :&#xA0; ($test2) ? &quot;TEST2 true&quot; : &quot;false&quot;;
?>
```

This will output: TEST2 true;

correct:



```
<?php
$test=true;
$test2=true;

($test) ? &quot;TEST1 true&quot; : (($test2) ? &quot;TEST2 true&quot; : &quot;false&quot;);
?>
```


Anyway don&apos;t nest them to much....!!

  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.comparison.php)

**[To root](/README.md)**