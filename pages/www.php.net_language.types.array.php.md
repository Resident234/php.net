# Arrays





please note that when arrays are copied, the &quot;reference status&quot; of their members is preserved (http://www.php.net/manual/en/language.references.whatdo.php).

  

#



I think your first, main example is needlessly confusing, very confusing to newbies:

$array = array(
&#xA0; &#xA0; &quot;foo&quot; =&gt; &quot;bar&quot;,
&#xA0; &#xA0; &quot;bar&quot; =&gt; &quot;foo&quot;,
);

It should be removed.
 
For newbies:
An array index can be any string value, even a value that is also a value in the array. 
The value of array[&quot;foo&quot;] is &quot;bar&quot;.
The value of array[&quot;bar&quot;] is &quot;foo&quot;

The following expressions are both true:
$array[&quot;foo&quot;] == &quot;bar&quot;
$array[&quot;bar&quot;] == &quot;foo&quot;

  

#



Beware that if you&apos;re using strings as indices in the $_POST array, that periods are transformed into underscores:

&lt;html&gt;
&lt;body&gt;


```
<?php
&#xA0; &#xA0; printf(&quot;POST: &quot;); print_r($_POST); printf(&quot;&lt;br/&gt;&quot;);
?>
```

&lt;form method=&quot;post&quot; action=&quot;

```
<?php echo $_SERVER[&apos;PHP_SELF&apos;]; ?>
```
&quot;&gt;
&#xA0; &#xA0; &lt;input type=&quot;hidden&quot; name=&quot;Windows3.1&quot; value=&quot;Sux&quot;&gt;
&#xA0; &#xA0; &lt;input type=&quot;submit&quot; value=&quot;Click&quot; /&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;

Once you click on the button, the page displays the following:

POST: Array ( [Windows3_1] =&gt; Sux )

  

#



Note that array value buckets are reference-safe, even through serialization.





```
<?php

$x=&apos;initial&apos;;

$test=array(&apos;A&apos;=&gt;&amp;$x,&apos;B&apos;=&gt;&amp;$x);

$test=unserialize(serialize($test));

$test[&apos;A&apos;]=&apos;changed&apos;;

echo $test[&apos;B&apos;]; // Outputs &quot;changed&quot;

?>
```




This can be useful in some cases, for example saving RAM within complex structures.

  

#



Since PHP 7.1, the string will not be converted to array automatically.

Below codes will fail:

$a=array();
$a[&apos;a&apos;]=&apos;&apos;;
$a[&apos;a&apos;][&apos;b&apos;]=&apos;&apos;;&#xA0; 
//Warning: Illegal string offset &apos;b&apos;
//Warning: Cannot assign an empty string to a string offset

You have to change to as below:

$a=array();

$a[&apos;a&apos;]=array(); // Declare it is an array first
$a[&apos;a&apos;][&apos;b&apos;]=&apos;&apos;;

  

#



&quot;If you convert a NULL value to an array, you get an empty array.&quot;



This turns out to be a useful property. Say you have a search function that returns an array of values on success or NULL if nothing found.





```
<?php $values = search(...); ?>
```




Now you want to merge the array with another array. What do we do if $values is NULL? No problem:





```
<?php $combined = array_merge((array)$values, $other); ?>
```




Voila.

  

#



//array keys are always integer and string data type and array values are all data type
//type casting and overwriting(data type of array key)
//----------------------------------------------------
$arr = array(
1=&gt;&quot;a&quot;,//int(1)
&quot;3&quot;=&gt;&quot;b&quot;,//int(3)
&quot;08&quot;=&gt;&quot;c&quot;,//string(2)&quot;08&quot;
&quot;80&quot;=&gt;&quot;d&quot;,//int(80)
&quot;0&quot;=&gt;&quot;e&quot;,//int(0)
&quot;Hellow&quot;=&gt;&quot;f&quot;,//string(6)&quot;Hellow&quot;
&quot;10Hellow&quot;=&gt;&quot;h&quot;,//string(8)&quot;10Hellow&quot;
1.5=&gt;&quot;j&quot;,//int(1.5)
&quot;1.5&quot;=&gt;&quot;k&quot;,//string(3)&quot;1.5&quot;
0.0=&gt;&quot;l&quot;,//int(0)
false=&gt;&quot;m&quot;,//int(false)
true=&gt;&quot;n&quot;,//int(true)
&quot;true&quot;=&gt;&quot;o&quot;,//string(4)&quot;true&quot;
&quot;false&quot;=&gt;&quot;p&quot;,//string(5)&quot;false&quot;
null=&gt;&quot;q&quot;,//string(0)&quot;&quot;
NULL=&gt;&quot;r&quot;,//string(0)&quot;&quot; note null and NULL are same
&quot;NULL&quot;=&gt;&quot;s&quot;,//string(4)&quot;NULL&quot; ,,,In last element of multiline array,comma is better to used.
);
//check the data type name of key
foreach ($arr as $key =&gt; $value) {
var_dump($key);
echo &quot;&lt;br&gt;&quot;;
}

//NOte :array and object data type in keys are Illegal ofset.......

  

#



--- quote ---
Note:
Both square brackets and curly braces can be used interchangeably for accessing array elements
--- quote end ---

At least for php 5.4 and 5.6; if function returns an array, the curly brackets does not work directly accessing function result, eg. WillReturnArray(){1} . This will give &quot;syntax error, unexpected &apos;{&apos; in...&quot;.
Personally I use only square brackets, expect for accessing single char in string. Old habits...

  

#



Used to creating arrays like this in Perl?

@array = (&quot;All&quot;, &quot;A&quot;..&quot;Z&quot;);

Looks like we need the range() function in PHP:



```
<?php
$array = array_merge(array(&apos;All&apos;), range(&apos;A&apos;, &apos;Z&apos;));
?>
```


You don&apos;t need to array_merge if it&apos;s just one range:



```
<?php
$array = range(&apos;A&apos;, &apos;Z&apos;);
?>
```



  

#



Regarding the previous comment, beware of the fact that reference to the last value of the array remains stored in $value after the foreach:



```
<?php
foreach ( $arr as $key =&gt; &amp;$value )
{
&#xA0; &#xA0; $value = 1;
}

// without next line you can get bad results...
//unset( $value );

$value = 159;
?>
```


Now the last element of $arr has the value of &apos;159&apos;. If we remove the comment in the unset() line, everything works as expected ($arr has all values of &apos;1&apos;).

Bad results can also appear in nested foreach loops (the same reason as above).

So either unset $value after each foreach or better use the longer form:



```
<?php
foreach ( $arr as $key =&gt; $value )
{
&#xA0; &#xA0; $arr[ $key ] = 1;
}
?>
```



  

#



When creating arrays , if we have an element with the same value as another element from the same array, we would expect PHP instead of creating new zval container to increase the refcount and point the duplicate symbol to the same zval. This is true except for value type integer. 
Example:

$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; 23, &apos;too&apos; =&gt; 23 ];
xdebug_debug_zval( &apos;arr&apos; );

Output:
arr:

(refcount=2, is_ref=0)
array (size=3)
&#xA0; &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)
&#xA0; &apos;age&apos; =&gt; (refcount=0, is_ref=0)int 23
&#xA0; &apos;too&apos; =&gt; (refcount=0, is_ref=0)int 23

but :
$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; 23, &apos;too&apos; =&gt; &apos;23&apos; ];
xdebug_debug_zval( &apos;arr&apos; );

will produce:
arr:

(refcount=2, is_ref=0)
array (size=3)
&#xA0; &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)
&#xA0; &apos;age&apos; =&gt; (refcount=0, is_ref=0)int 23
&#xA0; &apos;too&apos; =&gt; (refcount=1, is_ref=0)string &apos;23&apos; (length=2)
or :

$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; [1,2], &apos;too&apos; =&gt; [1,2] ];
xdebug_debug_zval( &apos;arr&apos; );

Output:
arr:

(refcount=2, is_ref=0)
array (size=3)
&#xA0; &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)
&#xA0; &apos;age&apos; =&gt; (refcount=2, is_ref=0)
&#xA0; &#xA0; array (size=2)
&#xA0; &#xA0; &#xA0; 0 =&gt; (refcount=0, is_ref=0)int 1
&#xA0; &#xA0; &#xA0; 1 =&gt; (refcount=0, is_ref=0)int 2
&#xA0; &apos;too&apos; =&gt; (refcount=2, is_ref=0)
&#xA0; &#xA0; array (size=2)
&#xA0; &#xA0; &#xA0; 0 =&gt; (refcount=0, is_ref=0)int 1
&#xA0; &#xA0; &#xA0; 1 =&gt; (refcount=0, is_ref=0)int 2

  

#



[Editor&apos;s note: You can achieve what you&apos;re looking for by referencing $single, rather than copying it by value in your foreach statement. See http://php.net/foreach for more details.]



Don&apos;t know if this is known or not, but it did eat some of my time and maybe it won&apos;t eat your time now...



I tried to add something to a multidimensional array, but that didn&apos;t work at first, look at the code below to see what I mean:





```
<?php



$a1 = array( &quot;a&quot; =&gt; 0, &quot;b&quot; =&gt; 1 );

$a2 = array( &quot;aa&quot; =&gt; 00, &quot;bb&quot; =&gt; 11 );



$together = array( $a1, $a2 );



foreach( $together as $single ) {

&#xA0; &#xA0; $single[ &quot;c&quot; ] = 3 ;

}



print_r( $together ); 

/* nothing changed result is:

Array

(

&#xA0; &#xA0; [0] =&gt; Array

&#xA0; &#xA0; &#xA0; &#xA0; (

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; 0

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; 1

&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [1] =&gt; Array

&#xA0; &#xA0; &#xA0; &#xA0; (

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [aa] =&gt; 0

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [bb] =&gt; 11

&#xA0; &#xA0; &#xA0; &#xA0; )

) */



foreach( $together as $key =&gt; $value ) {

&#xA0; &#xA0; $together[$key][&quot;c&quot;] = 3 ;

}



print_r( $together );



/* now it works, this prints

Array

(

&#xA0; &#xA0; [0] =&gt; Array

&#xA0; &#xA0; &#xA0; &#xA0; (

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; 0

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; 1

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [c] =&gt; 3

&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [1] =&gt; Array

&#xA0; &#xA0; &#xA0; &#xA0; (

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [aa] =&gt; 0

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [bb] =&gt; 11

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [c] =&gt; 3

&#xA0; &#xA0; &#xA0; &#xA0; )

)

*/



?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.types.array.php)

**[To root](/README.md)**