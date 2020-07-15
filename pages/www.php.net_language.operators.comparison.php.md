# Comparison of floating point numbers



I couldn&apos;t find much info on stacking the new ternary operator, so I ran some tests:<br><br>

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
<br><br>It works just as expected, returning the first non-false value within a group of expressions.  

#

[Editor&apos;s note: consider using ===]<br><br>I discover after 10 years of PHP development something awfull : even if you make a string comparison (both are strings), strings are tested like integers and leading "space" character (even \n, \r, \t) is ignored ....<br><br>I spent hours because of leading \n in a string ... it hurts my developper sensibility to see two strings beeing compared like integers and not like strings ... I use strcmp now for string comparison ... so stupid ...<br><br>Test code :<br>

```
<?php

test("1234", "1234");
test("1234", " 1234");
test("1234", "\n1234");
test("1234", "1234 ");
test("1234", "1234\n");

function test($v1, $v2) {
    echo "<h1>[".show_cr($v1)."] vs [".show_cr($v2)."]</h1>";
    echo my_var_dump($v1)."<br />";
    echo my_var_dump($v2)."<br />";
    if($v1 == $v2) {
        echo "EQUAL !";
    }
    else {
        echo "DIFFERENT !";
    }
}

function show_cr($var) {
    return str_replace("\n", "\\n", $var);
}

function my_var_dump($var) {
    ob_start();
    var_dump($var);
    $dump = show_cr(trim(ob_get_contents()));
    ob_end_clean();
    return $dump;
}

?>
```
<br><br>Displays this -&gt;<br><br>[1234] vs [1234]<br>string(4) "1234"<br>string(4) "1234"<br>EQUAL !<br><br>[1234] vs [ 1234]<br>string(4) "1234"<br>string(5) " 1234"<br>EQUAL !<br><br>[1234] vs [\n1234]<br>string(4) "1234"<br>string(5) "\n1234"<br>EQUAL !<br><br>[1234] vs [1234 ]<br>string(4) "1234"<br>string(5) "1234 "<br>DIFFERENT !<br><br>[1234] vs [1234\n]<br>string(4) "1234"<br>string(5) "1234\n"<br>DIFFERENT !  

#

note: the behavior below is documented in the appendix K about type comparisons, but since it is somewhat buried i thought i should raise it here for people since it threw me for a loop until i figured it out completely.<br><br>just to clarify a tricky point about the == comparison operator when dealing with strings and numbers:<br><br>(&apos;some string&apos; == 0) returns TRUE<br><br>however, (&apos;123&apos; == 0) returns FALSE<br><br>also note that ((int) &apos;some string&apos;) returns 0<br><br>and ((int) &apos;123&apos;) returns 123<br><br>the behavior makes senes but you must be careful when comparing strings to numbers, e.g. when you&apos;re comparing a request variable which you expect to be numeric. its easy to fall into the trap of:<br><br>if ($_GET[&apos;myvar&apos;]==0) dosomething();<br><br>as this will dosomething() even when $_GET[&apos;myvar&apos;] is &apos;some string&apos; and clearly not the value 0<br><br>i was getting lazy with my types since php vars are so flexible, so be warned to pay attention to the details...  

#

I was interested about the following two uses of the ternary operator (PHP &gt;= 5.3) for using a "default" value if a variable is not set or evaluates to false:<br><br>

```
<?php
(isset($some_variable) &amp;&amp; $some_variable) ? $some_variable : 'default_value';

$some_variable ?: 'default_value';
?>
```
<br><br>The second is more readable, but will throw an ERR_NOTICE is $some_variable is not set. Of course, this could be overcome by suppressing the notice using the @ operator.<br><br>Performance-wise, though, comparing 1 million iterations of the three statements<br><br>  (isset($foo) &amp;&amp; $foo) ? $foo : &apos;&apos;<br>  ($foo) ?: &apos;&apos;<br>  (@$foo) ?: &apos;&apos;<br><br>results in the following:<br><br>  $foo is NOT SET.<br>    [isset] 0.18222403526306<br>    [?:]    0.57496404647827<br>    [@ ?:]  0.64780592918396<br>  $foo is NULL.<br>    [isset] 0.17995285987854<br>    [?:]    0.15304207801819<br>    [@ ?:]  0.20394206047058<br>  $foo is FALSE.<br>    [isset] 0.19388508796692<br>    [?:]    0.15359902381897<br>    [@ ?:]  0.20741701126099<br>  $foo is TRUE.<br>    [isset] 0.17265486717224<br>    [?:]    0.11773896217346<br>    [@ ?:]  0.16193103790283<br><br>In other words, using the long-form ternary operator with isset($some_variable) is preferable overall if $some_variable may not be set.<br><br>(error_reporting was set to zero for the benchmark, to avoid printing a million notices...)  

#

Be careful when using the ternary operator!<br><br>The following will not evaluate to the expected result:<br><br>

```
<?php
echo "a string that has a " . (true) ? 'true' : 'false' . " condition in. ";
?>
```


Will print true.

Instead, use this:



```
<?php
echo "a string that has a " . ((true) ? 'true' : 'false') . " condition in. ";
?>
```
<br><br>This will evaluate to the expected result: "a string that has a true condition in. "<br><br>I hope this helps.  

#

Be careful with the "==" operator when both operands are strings:<br>

```
<?php
var_dump('123' == '       123'); // true
var_dump('1e3' == '1000'); // true
var_dump('+74951112233' == '74951112233'); // true
var_dump('00000020' == '0000000000000000020'); // true
var_dump('0X1D' == '29E0'); // true
var_dump('0xafebac' == '11529132'); // true
var_dump('0xafebac' == '0XAFEBAC'); // true
var_dump('0xeb' == '+235e-0'); // true
var_dump('0.235' == '+.235'); // true
var_dump('0.2e-10' == '2.0E-11'); // true
var_dump('61529519452809720693702583126814' == '61529519452809720000000000000000'); // true in php < 5.4.4?>
```
  

#

The following contrasts the trinary operator associativity in PHP and Java.  The first test would work as expected in Java (evaluates left-to-right, associates right-to-left, like if stmnt), the second in PHP (evaluates and associates left-to-right)<br><br>

```
<?php

echo "\n\n######----------- trinary operator associativity\n\n";

function trinaryTest($foo){

    $bar    = $foo > 20
            ? "greater than 20"
            : $foo > 10
                ? "greater than 10"
                : $foo > 5
                    ? "greater than 5"
                    : "not worthy of consideration";    
    echo $foo." =>  ".$bar."\n";
}

echo "----trinaryTest\n\n";
trinaryTest(21);
trinaryTest(11);
trinaryTest(6);
trinaryTest(4);

function trinaryTestParens($foo){
    
    $bar    = $foo > 20
            ? "greater than 20"
            : ($foo > 10
                ? "greater than 10"
                : ($foo > 5
                    ? "greater than 5"
                    : "not worthy of consideration"));    
    echo $foo." =>  ".$bar."\n";
}

echo "----trinaryTestParens\n\n";
trinaryTestParens(21);
trinaryTestParens(11);
trinaryTest(6);
trinaryTestParens(4);

?>
```
<br><br>Output:<br><br>######----------- trinary operator associativity <br><br>----trinaryTest<br><br>21 =&gt;  greater than 5<br>11 =&gt;  greater than 5<br>6 =&gt;  greater than 5<br>4 =&gt;  not worthy of consideration<br><br>----trinaryTestParens<br><br>21 =&gt;  greater than 20<br>11 =&gt;  greater than 10<br>6 =&gt;  greater than 5<br>4 =&gt;  not worthy of consideration  

#

if you want to use the ?: operator, you should be careful with the precedence.<br><br>Here&apos;s an example of the priority of operators:<br><br>

```
<?php
echo 'Hello, ' . isset($i) ? 'my friend: ' . $username . ', how are you doing?' : 'my guest, ' . $guestusername . ', please register';
?>
```


This make "'Hello, ' . isset($i)" the sentence to evaluate. So, if you think to mix more sentences with the ?: operator, please use always parentheses to force the proper evaluation of the sentence.



```
<?php
echo 'Hello, ' . (isset($i) ? 'my friend: ' . $username . ', how are you doing?' : 'my guest, ' . $guestusername . ', please register');
?>
```
<br><br>for general rule, if you mix ?: with other sentences, always close it with parentheses.  

#

For converted Perl programmers: use strict comparison operators (===, !==) in place of string comparison operators (eq, ne). Don&apos;t use the simple equality operators (==, !=), because ($a == $b) will return TRUE in many situations where ($a eq $b) would return FALSE.<br><br>For instance...<br>"mary" == "fred" is FALSE, but<br>"+010" == "10.0" is TRUE (!)<br><br>In the following examples, none of the strings being compared are identical, but because PHP *can* evaluate them as numbers, it does so, and therefore finds them equal...<br><br>

```
<?php

echo ("007" == "7" ? "EQUAL" : "not equal");
// Prints: EQUAL

// Surrounding the strings with single quotes (') instead of double
// quotes (") to ensure the contents aren't evaluated, and forcing
// string types has no effect.
echo ( (string)'0001' == (string)'+1.' ? "EQUAL" : "not equal");
// Prints: EQUAL

// Including non-digit characters (like leading spaces, "e", the plus
// or minus sign, period, ...) can still result in this behavior, if
// a string happens to be valid scientific notation.
echo ('  131e-2' == '001.3100' ? "EQUAL" : "not equal");
// Prints: EQUAL

?>
```


If you're comparing passwords (or anything else for which "near" precision isn't good enough) this confusion could be detrimental. Stick with strict comparisons...



```
<?php

// Same examples as above, using === instead of ==

echo ("007" === "7" ? "EQUAL" : "not equal");
// Prints: not equal

echo ( (string)'0001' === (string)'+1.' ? "EQUAL" : "not equal");
// Prints: not equal

echo ('  131e-2' === '001.3100' ? "EQUAL" : "not equal");
// Prints: not equal

?>
```
  

#

A quick way to do mysql bit comparison in php is to use the special character it stores . e.g<br>

```
<?php
                                        if ($AvailableRequests['OngoingService'] == '')
                                            echo '<td>Yes</td>';
                                        else
                                            echo '<td>No</td>';

?>
```
  

#

Note: according to the spec, PHP&apos;s comparison operators are not transitive.  For example, the following are all true in PHP5:<br><br>"11" &lt; "a" &lt; 2 &lt; "11"<br><br>As a result, the outcome of sorting an array depends on the order the elements appear in the pre-sort array.  The following code will dump out two arrays with *different* orderings:<br><br>

```
<?php
$a = array(2,    "a",  "11", 2);
$b = array(2,    "11", "a",  2);
sort($a);
var_dump($a);
sort($b);
var_dump($b);
?>
```
<br><br>This is not a bug report -- given the spec on this documentation page, what PHP does is "correct".  But that may not be what was intended...  

#

Beware of the consequences of comparing strings to numbers.  You can disprove the laws of the universe.<br><br>echo (&apos;X&apos; == 0 &amp;&amp; &apos;X&apos; == true &amp;&amp; 0 == false) ? &apos;true == false&apos; : &apos;sanity prevails&apos;;<br><br>This will output &apos;true == false&apos;.  This stems from the use of the UNIX function strtod() to convert strings to numbers before comparing.  Since &apos;X&apos; or any other string without a number in it converts to 0 when compared to a number, 0 == 0 &amp;&amp; &apos;X&apos; == true &amp;&amp; 0 == false  

#

You can&apos;t just compare two arrays with the === operator<br>like you would think to find out if they are equal or not.  This is more complicated when you have multi-dimensional arrays.  Here is a recursive comparison function.<br><br>

```
<?php
/**
 * Compares two arrays to see if they contain the same values.  Returns TRUE or FALSE.
 * usefull for determining if a record or block of data was modified (perhaps by user input)
 * prior to setting a "date_last_updated" or skipping updating the db in the case of no change.
 *
 * @param array $a1
 * @param array $a2
 * @return boolean
 */
function array_compare_recursive($a1, $a2)
{
   if (!(is_array($a1) and (is_array($a2)))) { return FALSE;}    
    
   if (!count($a1) == count($a2)) 
      {
       return FALSE; // arrays don't have same number of entries
      }
      
   foreach ($a1 as $key => $val) 
   {
       if (!array_key_exists($key, $a2)) 
           {return FALSE; // uncomparable array keys don't match
              } 
       elseif (is_array($val) and is_array($a2[$key]))  // if both entries are arrays then compare recursive 
           {if (!array_compare_recursive($val,$a2[$key])) return FALSE;
           } 
       elseif (!($val === $a2[$key])) // compare entries must be of same type.
           {return FALSE;
           }
   }
   return TRUE; // $a1 === $a2
}
?>
```
  

#

I think everybody should read carefully what "jeronimo at DELETE_THIS dot transartmedia dot com" wrote. It&apos;s a great pitfall even for seasoned programmers and should be looked upon with a great attention.<br>For example, comparing passwords with == may result in a very large security hole.<br><br>I would add some more to it:<br><br>The workaround is to use strcmp() or ===.<br><br>Note on ===:<br><br>While the php documentation says that, basically,<br>($a===$b)  is the same as  ($a==$b &amp;&amp; gettype($a) == gettype($b)),<br>this is not true.<br><br>The difference between == and === is that === never does any type conversion. So, while, according to documentation, ("+0.1" === ".1") should return true (because both are strings and == returns true), === actually returns false (which is good).  

#

beware of the fact, that there is no `&lt;==` nor `&gt;==` therefore `false &lt;= 0` will be `true`. php v. 5.4.27  

#

When you want to know if two arrays contain the same values, regardless of the values&apos; order, you cannot use "==" or "===".  In other words:<br><br>

```
<?php
(array(1,2) == array(2,1)) === false;
?>
```


To answer that question, use:



```
<?php
function array_equal($a, $b) {
    return (is_array($a) &amp;&amp; is_array($b) &amp;&amp; array_diff($a, $b) === array_diff($b, $a));
}
?>
```


A related, but more strict problem, is if you need to ensure that two arrays contain the same key=>value pairs, regardless of the order of the pairs.  In that case, use:



```
<?php
function array_identical($a, $b) {
    return (is_array($a) &amp;&amp; is_array($b) &amp;&amp; array_diff_assoc($a, $b) === array_diff_assoc($b, $a));
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

$a = array ('a' => 2, 'b' => 1);
$b = array ('b' => 1, 'a' => 2);
// true === array_identical($a, $b)
// true === array_equal($a, $b)
?>
```
<br><br>(See also the solution "rshawiii at yahoo dot com" posted)  

#

In the table "Comparison with Various Types", please move the last line about "Object" to be above the line about "Array", since Object is considered to be greater than Array (tested on 5.3.3)<br><br>(Please remove my "Anonymous" post of the same content before. You could check IP to see that I forgot to type my name)  

#

Note that typecasting will NOT prevent the default behavior for converting two numeric strings to numbers when comparing them.<br><br>e.g.:<br><br>

```
<?php
if ((string) '0123' == (string) '123')
    print 'equals';
else
    print 'doesn\'t equal';
?>
```
<br><br>Still prints &apos;equals&apos;<br><br>As far as I can tell the only way to avoid this is to use the identity comparison operators (=== and !==).  

#

Note: The ternary shortcut currently seems to be of no use in dealing with unexisting keys in an array, as PHP will throw an error. Take the following example.<br><br>

```
<?php
$_POST['Unexisting'] = $_POST['Unexisting'] ?: false;
?>
```
<br><br>PHP will throw an error that the "Unexisting" key does not exist. The @ operator does not work here to suppress this error.  

#

a function to help settings default values, it returns its own first non-empty argument :<br><br>make your own eor combos !<br><br>

```
<?php

/*
 * Either Or
 *
 * usage:  $foo = eor(test1(),test2(),"default");
 * usage:  $foo = eor($_GET['foo'], foogen(), $foo, "bar");
 */

function eor() {
    $vars = func_get_args();
     while (!empty($vars) &amp;&amp; empty($defval))    
         $defval = array_shift($vars);          
     return $defval;
}

 

?>
```
  

#

If you need nested ifs on I var its important to group the if so it works.<br>Example:<br>

```
<?php
//Dont Works
//Parse error: parse error, unexpected ':' 
 $var='<option value="1" '.$status == "1" ? 'selected="selected"' :''.'>Value 1</option>';
 //Works:
 $var='<option value="1" '.($status == "1" ? 'selected="selected"' :'').'>Value 1</option>';

echo $var;
?>
```
  

#

Note that the "ternary operator" is better described as the "conditional operator". The former name merely notes that it has three arguments without saying anything about what it does. Needless to say, if PHP picked up any more ternary operators, this will be a problem.<br><br>"Conditional Operator" is actually descriptive of the semantics, and is the name historically given to it in, e.g., C.  

#

I came across peculiar outputs while I was attempting to debug a script<br><br>

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
    [1] => 1
)
*/

# Works: This is what I need/expect
# $tally[$index] = 1+(isset($tally[$index])?$tally[$index]:0);

print_r($tally);
?>
```
<br><br>The second block obviously does not work what one expects.<br>Third part is good.  

#

With Nested ternary Operators you have to set the logical  parentheses to get the correct result.<br><br>

```
<?php
$test=true;
$test2=true;

($test) ? "TEST1 true" :  ($test2) ? "TEST2 true" : "false";
?>
```

This will output: TEST2 true;

correct:



```
<?php
$test=true;
$test2=true;

($test) ? "TEST1 true" : (($test2) ? "TEST2 true" : "false");
?>
```
<br><br>Anyway don&apos;t nest them to much....!!  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.comparison.php)

**[To root](/README.md)**