# unset



This is probably trivial but there is no error for unsetting a non-existing variable.  

#

You don&apos;t need to check that a variable is set before you unset it.<br>

```
<?php
unset($a);
?>
```

is harmless.


```
<?php
if(isset($a)) {
    unset($a);
}
?>
```
<br>is pointless complication.<br><br>This doesn&apos;t apply to properties of objects that have __isset() methods that visibly change object state or __unset() methods that don&apos;t properly check their arguments or have extra side effects.<br><br>The latter case means that __unset shouldn&apos;t do more than what it says on the tin, and also has the responsibility for checking (possibly using __isset()) that what it&apos;s being asked to do makes sense.<br><br>The former case is just plain bad design.  

#

Since unset() is a language construct, it cannot be passed anything other than a variable. It&apos;s sole purpose is to "unset" this variable, ie. to remove it from the current scope and destroy it&apos;s associated data. This is true especially for reference variables, where not the actual value is destroyed but the reference to that value. This is why you can&apos;t wrap &apos;unset()&apos; in a user defined function: You would either unset a copy of the data if the parameter is passed by value, or you would just unset the reference variable within the functions scope if the parameter is passed by reference. There is no workaround for that, as you cannot pass &apos;scope&apos; to a function in PHP. Such a function can only work for variables that exist in a common or global scope (compare &apos;unset($_GLOBALS[variable])&apos;).<br><br>I don&apos;t know how PHP handles garbage collection internally, but I guess this behavior can result in a huge memory leak: if a value variable goes out of scope with a second variable still holding a reference to the in-memory value, then unsetting that reference would still hold the value in memory but potentially unset the last reference to that in-memory data, hence: occupied memory that is rendered useless as you cannot reference it anymore.  

#

if you try to unset an object, please be careful about references.<br><br>Objects will only free their resources and trigger their __destruct method when *all* references are unsetted.<br>Even when they are *in* the object... sigh!<br><br>

```
<?php

class A {
  function __destruct() {
    echo "cYa later!!\n";
  }
}

$a = new A();
$a -&gt; a = $a;
#unset($a); # Just uncomment, and you&apos;ll see

echo "No Message ... hm, what now?\n";
unset($a -&gt; a);
unset($a);

echo "Finally that thing is gone\n";

?>
```
<br><br>Of course the object completely dies at the end of the script.  

#

A sample how to unset array elements from an array result coming from a mysql request. In this sample it is checking if a file exists and removes the row from the array if it not exists.<br><br>

```
<?php
$db-&gt;set_query("select * from documents where document_in_user = 0"); //1  
$documents = $db-&gt;result_to_array($db-&gt;get_result()); //1

foreach ($documents as $key =&gt; $row) { //2

    $file     = "uploads/".rawurldecode($row[&apos;document_name&apos;]);
  
    if ( file_exists ( $file ) == FALSE ) {
         unset($documents[$key]);  //3
    }  
}

$documents = array_values($documents); // reindex the array (4)
?>
```
<br><br>variables:<br>mysql table = documents,<br>array = $documents<br>array key (index) = $key<br>array row (record sort of speak) = $row<br><br>explanation:<br><br>1. <br>it gets the array from the table (mysql)<br><br>2. <br>foreach goes through the array $documents<br><br>3. <br>unset if record does not exist<br><br>4.<br>the array_values($documents) reindexes the $documents array, for otherwise you might end up in trouble when your  process will start expecting an array starting with key ($key) 0 (zero).  

#

Here is another way to make &apos;unset&apos; work with session variables from within a function : <br><br>

```
<?php
function unsetSessionVariable ($sessionVariableName) {
   unset($GLOBALS[_SESSION][$sessionVariableName]);
}
?>
```
<br><br>May it work with others than me...<br>F.  

#

Only This works with register_globals being &apos;ON&apos;.<br><br>unset( $_SESSION[&apos;variable&apos;] );<br><br>The above will not work with register_globals turned on (will only work outside of a function).<br><br>$variable = $_SESSION[&apos;variable&apos;];<br>unset( $_SESSION[&apos;variable&apos;], $variable );<br><br>The above will work with register_globals on &amp; inside a function  

#

Adding on to what bond at noellebond dot com said, if you want to remove an index from the end of the array, if you use unset, the next index value will still be what it would have been.<br><br>Eg you have <br>

```
<?php
 $x = array(1, 2);

 for ($i = 0; $i &lt; 5; $i++)
 {
    unset($x[(count($x)-1)]); //remove last set key in the array

    $x[] = $i;
 }
?>
```


You would expect:
Array([0] =&gt; 1, [1] =&gt; 4)
as you want it to remove the last set key....

but you actually get
Array ( [0] =&gt; 1 [4] =&gt; 2 [5] =&gt; 3 [6] =&gt; 4 ) 

This is since even though the last key is removed, the auto indexing still keeps its previous value.

The only time where this would not seem right is when you remove a value off the end. I guess different people would want it different ways.

The way around this is to use array_pop() instead of unset() as array_pop() refreshes the autoindexing thing for the array.


```
<?php
 $x = array(1, 2);

 for ($i = 0; $i &lt; 5; $i++)
 {
    array_pop($x); // removes the last item in the array

    $x[] = $i;
 }
?>
```
<br><br> This returns the expected value of x = Array([0] =&gt; 1, [1] =&gt; 4);<br><br>Hope this helps someone who may need this for some odd reason, I did.  

#

To clarify what hugo dot dworak at gmail dot com said about unsetting things that aren&apos;t already set:<br><br>unsetting a non-existent key within an array does NOT throw an error.<br>&lt;?<br>$array = array();<br><br>unset($array[2]);<br>//this does not throw an error<br><br>unset($array[$undefinedVar]);<br>//Throws an error because of the undefined variable, not because of a non-existent key.<br>?>
```
  

#

In addition to what timo dot hummel at 4fb dot de said;<br><br>&gt;For the curious: unset also frees memory of the variable used.<br>&gt;<br>&gt;It might be possible that the in-memory size of the PHP Interpreter isn&apos;t reduced, but your scripts won&apos;t touch the memory_limit boundary. Memory is reused if you declare new variables.<br><br>It might be worth adding that functions apparently don&apos;t free up memory on exit the same way unset does..<br>Maybe this is common knowledge, but although functions destroys variables on exit, it (apparently) doesn&apos;t help the memory.<br><br>So if you use huge variables inside functions, be sure to unset them if you can before returning from the function.<br><br>In my case, if I did not unset before return, then the script would use 20 MB more of memory than if I did unset.<br>This was tested with php 5.0.4 on apache 2 on windows xp, with no memory limit.<br><br>Before I did the test, I was under the impression that when you exit from functions, the memory used inside it would be cleared and reused. Maybe this should be made clear in the manual, for either unset() or in the chapter for functions.  

#

Despite much searching, I have not yet found an explanation as to how one can manually free resources from variables, not so much objects, in PHP.  I have also seen many comments regarding the merits and demerits of unset() versus setting a variable to null.  Thus, here are the results of some benchmarks performed comparing unset() of numerous variables to setting them to null (with regards to memory usage and processing time):<br><br>10 variables:<br>Unset:<br>Memory Usage: 296<br>Time Elapsed: 1.0013580322266E-5<br><br>Null set:<br>Memory Usage: 1736<br>Time Elapsed: 5.9604644775391E-6<br><br>50 variables:<br>Unset:<br>Memory Usage: 296<br>Time Elapsed: 3.6001205444336E-5<br><br>Null set:<br>Memory Usage: 8328<br>Time Elapsed: 3.2901763916016E-5<br><br>100 variables:<br>Unset:<br>Memory Usage: 296<br>Time Elapsed: 5.6982040405273E-5<br><br>Null set:<br>Memory Usage: 15928<br>Time Elapsed: 5.8174133300781E-5<br><br>1000 variables:<br>Unset:<br>Memory Usage: 296<br>Time Elapsed: 0.00041294097900391<br><br>Null set:<br>Memory Usage: 168096<br>Time Elapsed: 0.00067591667175293<br><br>10000 variables:<br>Unset:<br>Memory Usage: 296<br>Time Elapsed: 0.0042569637298584<br><br>Null set:<br>Memory Usage: 1650848<br>Time Elapsed: 0.0076930522918701<br><br>100000 variables:<br>Unset:<br>Memory Usage: 296<br>Time Elapsed: 0.042603969573975<br><br>Null set:<br>Memory Usage: 16249080<br>Time Elapsed: 0.087724924087524<br><br>300000 variables:<br>Unset:<br>Memory Usage: 296<br>Time Elapsed: 0.13177299499512<br><br>Null set:<br>Memory Usage: 49796320<br>Time Elapsed: 0.28617882728577<br><br>Perhaps my test code for the null set was flawed, but despite that possibility it is simple to see that unset() has minimal processing time impact, and no apparent memory usage impact (unless the values returned by memory_get_usage() are flawed).  If you truly care about the ~4 microseconds saved over &lt;50 variables, more power to you.  Otherwise, use unset() to minimize script impact on your system.<br>Note: Tested on PHP 5.3.8 installed via RPM on Fedora 14  

#

Note that PHP 4 will generate a warning if you try to unset an array index that doesn&apos;t exist and whose parent doesn&apos;t exist.<br><br>Example:<br><br>

```
<?php

  $foo = array();

  unset($foo[&apos;Bar&apos;][&apos;Baz&apos;]);

?>
```
<br><br>RESULT: "Notice:  Undefined index:  Bar"<br><br>On PHP5 no error is raised, which seems to me like the correct behaviour.<br><br>Note that using unset($foo[&apos;Bar&apos;]) in the above example does not generate a warning in either version.<br><br>(Tested on 4.4.9 and 5.2.4)  

#

Just to confirm, USING UNSET CAN DESTROY AN ENTIRE ARRAY. I couldn&apos;t find reference to this anywhere so I decided to write this. <br><br>The difference between using unset and using $myarray=array(); to unset is that obviously the array will just be overwritten and will still exist.<br><br>

```
<?php

$myarray=array("Hello","World");

echo $myarray[0].$myarray[1];

unset($myarray);
//$myarray=array();

echo $myarray[0].$myarray[1];

echo $myarray;
?>
```


Output with unset is:
&lt;?
HelloWorld

Notice: Undefined offset: 0 in C:\webpages\dainsider\myarray.php on line 10

Notice: Undefined offset: 1 in C:\webpages\dainsider\myarray.php on line 10

Output with $myarray=array(); is:
?>
```


&lt;?
HelloWorld

Notice: Undefined offset: 0 in C:\webpages\dainsider\myarray.php on line 10

Notice: Undefined offset: 1 in C:\webpages\dainsider\myarray.php on line 10

Array
?>
```
  

#

In PHP 5.0.4, at least, one CAN unset array elements inside functions from arrays passed by reference to the function.<br>As implied by the manual, however, one can&apos;t unset the entire array by passing it by reference.<br><br>

```
<?php
function remove_variable (&amp;$variable)  // pass variable by reference
{
    unset($variable);
}

function remove_element (&amp;$array, $key) // pass array by reference
{
    unset($array[$key]);
}

$scalar = &apos;Hello, there&apos;;
echo &apos;Value of $scalar is: &apos;;
print_r ($scalar); echo &apos;&lt;br /&gt;&apos;;
// Value of $scalar is: Hello, there

remove_variable($scalar); // try to unset the variable
echo &apos;Value of $scalar is: &apos;;
print_r ($scalar); echo &apos;&lt;br /&gt;&apos;;
// Value of $scalar is: Hello, there

$array = array(&apos;one&apos; =&gt; 1, &apos;two&apos; =&gt; 2, &apos;three&apos; =&gt; 3);
echo &apos;Value of $array is: &apos;;
print_r ($array); echo &apos;&lt;br /&gt;&apos;;
// Value of $array is: Array ( [one] =&gt; 1 [two] =&gt; 2 [three] =&gt; 3 )

remove_variable($array); // try to unset the array
echo &apos;Value of $array is: &apos;;
print_r ($array); echo &apos;&lt;br /&gt;&apos;;
// Value of $array is: Array ( [one] =&gt; 1 [two] =&gt; 2 [three] =&gt; 3 )

remove_element($array, &apos;two&apos;); // successfully remove an element from the array
echo &apos;Value of $array is: &apos;;
print_r ($array); echo &apos;&lt;br /&gt;&apos;;
// Value of $array is: Array ( [one] =&gt; 1 [three] =&gt; 3 )

?>
```
  

#

Here&apos;s my variation on the slightly dull unset method. It throws in a bit of 80&apos;s Stallone action spice into the mix. Enjoy!<br><br>

```
<?php
/**
 * function rambo (first blood)
 *
 * Completely and utterly destroys everything, returning the kill count of victims
 *
 * @param    It don&apos;t matter, it&#x2019;s Rambo baby
 * @return    Integer    Body count (but any less than 500 and it&apos;s not really worth mentioning)
 */
function rambo() {

    // Get the victims and initiate that body count status
    $victims = func_get_args();
    $body_count = 0;    
    
    // Kill those damn punks
    foreach($victims as $victim) {
        if($death_and_suffering = @unset($victim)) {
            $body_count++;
        }
    }
    
    // How many kills did Rambo tally up on this mission?
    return($body_count);
}
?>
```
  

#

The documentation is not entirely clear when it comes to static variables. It says:<br><br>If a static variable is unset() inside of a function, unset() destroys the variable and all its references. <br><br>

```
<?php
function foo() 
{
   static $a;
   $a++;
   echo "$a\n";
   unset($a);
}

foo();
foo();
foo();
?>
```
  

The above example would output: 

1
2
3

And it does! But the variable is NOT deleted, that&apos;s why the value keeps on increasing, otherwise the output would be:

1
1
1 

The references are destroyed within the function, this handeling is the same as with global variables, the difference is a static variable is a local variable.

Be carefull using unset and static values as the output may not be what you expect it to be. It appears to be impossible to destroy a static variable. You can only destroy the references within the current executing function, a successive static statement will restore the references.

The documentation would be better if it would say:
"If a static variable is unset() inside of a function, unset() destroys all references to the variable. "

Example: (tested PHP 4.3.7)


```
<?php
function foo() 
{
   static $a;
   $a++;
   echo "$a\n";
   unset($a);
   echo "$a\n";
   static $a;    
   echo "$a\n";
}

foo();
foo();
foo();
?>
```
 <br><br>Would output:<br><br>1<br><br>1<br>2<br><br>2<br>3<br><br>3  

#

dh at argosign dot de - <br>it is possible to unset globals from within functions thanks to the $GLOBALS array:<br><br>

```
<?php
$x = 10;

function test() {
    // don&apos;t need to do &apos; global $x; &apos;
    unset ($GLOBALS[&apos;x&apos;]);
    echo &apos;x: &apos; . $GLOBALS[&apos;x&apos;] . &apos;&lt;br /&gt;&apos;;
}

test();
echo "x: $x&lt;br /&gt;";

// will result in
/*
x: 
x:
*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.unset.php)

**[To root](/README.md)**