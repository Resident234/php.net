# Arrays



please note that when arrays are copied, the "reference status" of their members is preserved (http://www.php.net/manual/en/language.references.whatdo.php).  

#

I think your first, main example is needlessly confusing, very confusing to newbies:<br><br>$array = array(<br>    "foo" =&gt; "bar",<br>    "bar" =&gt; "foo",<br>);<br><br>It should be removed.<br> <br>For newbies:<br>An array index can be any string value, even a value that is also a value in the array. <br>The value of array["foo"] is "bar".<br>The value of array["bar"] is "foo"<br><br>The following expressions are both true:<br>$array["foo"] == "bar"<br>$array["bar"] == "foo"  

#

Beware that if you&apos;re using strings as indices in the $_POST array, that periods are transformed into underscores:<br><br>&lt;html&gt;<br>&lt;body&gt;<br>

```
<?php
    printf("POST: "); print_r($_POST); printf("<br/>");
?>
```

<form method="post" action="

```
<?php echo $_SERVER['PHP_SELF']; ?>
```
"&gt;<br>    &lt;input type="hidden" name="Windows3.1" value="Sux"&gt;<br>    &lt;input type="submit" value="Click" /&gt;<br>&lt;/form&gt;<br>&lt;/body&gt;<br>&lt;/html&gt;<br><br>Once you click on the button, the page displays the following:<br><br>POST: Array ( [Windows3_1] =&gt; Sux )  

#

Note that array value buckets are reference-safe, even through serialization.<br><br>

```
<?php
$x='initial';
$test=array('A'=>&amp;$x,'B'=>&amp;$x);
$test=unserialize(serialize($test));
$test['A']='changed';
echo $test['B']; // Outputs "changed"
?>
```
<br><br>This can be useful in some cases, for example saving RAM within complex structures.  

#

Since PHP 7.1, the string will not be converted to array automatically.<br><br>Below codes will fail:<br><br>$a=array();<br>$a[&apos;a&apos;]=&apos;&apos;;<br>$a[&apos;a&apos;][&apos;b&apos;]=&apos;&apos;;  <br>//Warning: Illegal string offset &apos;b&apos;<br>//Warning: Cannot assign an empty string to a string offset<br><br>You have to change to as below:<br><br>$a=array();<br><br>$a[&apos;a&apos;]=array(); // Declare it is an array first<br>$a[&apos;a&apos;][&apos;b&apos;]=&apos;&apos;;  

#

"If you convert a NULL value to an array, you get an empty array."<br><br>This turns out to be a useful property. Say you have a search function that returns an array of values on success or NULL if nothing found.<br><br>

```
<?php $values = search(...); ?>
```


Now you want to merge the array with another array. What do we do if $values is NULL? No problem:



```
<?php $combined = array_merge((array)$values, $other); ?>
```
<br><br>Voila.  

#

//array keys are always integer and string data type and array values are all data type<br>//type casting and overwriting(data type of array key)<br>//----------------------------------------------------<br>$arr = array(<br>1=&gt;"a",//int(1)<br>"3"=&gt;"b",//int(3)<br>"08"=&gt;"c",//string(2)"08"<br>"80"=&gt;"d",//int(80)<br>"0"=&gt;"e",//int(0)<br>"Hellow"=&gt;"f",//string(6)"Hellow"<br>"10Hellow"=&gt;"h",//string(8)"10Hellow"<br>1.5=&gt;"j",//int(1.5)<br>"1.5"=&gt;"k",//string(3)"1.5"<br>0.0=&gt;"l",//int(0)<br>false=&gt;"m",//int(false)<br>true=&gt;"n",//int(true)<br>"true"=&gt;"o",//string(4)"true"<br>"false"=&gt;"p",//string(5)"false"<br>null=&gt;"q",//string(0)""<br>NULL=&gt;"r",//string(0)"" note null and NULL are same<br>"NULL"=&gt;"s",//string(4)"NULL" ,,,In last element of multiline array,comma is better to used.<br>);<br>//check the data type name of key<br>foreach ($arr as $key =&gt; $value) {<br>var_dump($key);<br>echo "&lt;br&gt;";<br>}<br><br>//NOte :array and object data type in keys are Illegal ofset.......  

#

--- quote ---<br>Note:<br>Both square brackets and curly braces can be used interchangeably for accessing array elements<br>--- quote end ---<br><br>At least for php 5.4 and 5.6; if function returns an array, the curly brackets does not work directly accessing function result, eg. WillReturnArray(){1} . This will give "syntax error, unexpected &apos;{&apos; in...".<br>Personally I use only square brackets, expect for accessing single char in string. Old habits...  

#

Used to creating arrays like this in Perl?<br><br>@array = ("All", "A".."Z");<br><br>Looks like we need the range() function in PHP:<br><br>

```
<?php
$array = array_merge(array('All'), range('A', 'Z'));
?>
```


You don't need to array_merge if it's just one range:



```
<?php
$array = range('A', 'Z');
?>
```
  

#

Regarding the previous comment, beware of the fact that reference to the last value of the array remains stored in $value after the foreach:<br><br>

```
<?php
foreach ( $arr as $key => &amp;$value )
{
    $value = 1;
}

// without next line you can get bad results...
//unset( $value );

$value = 159;
?>
```


Now the last element of $arr has the value of '159'. If we remove the comment in the unset() line, everything works as expected ($arr has all values of '1').

Bad results can also appear in nested foreach loops (the same reason as above).

So either unset $value after each foreach or better use the longer form:



```
<?php
foreach ( $arr as $key => $value )
{
    $arr[ $key ] = 1;
}
?>
```
  

#

When creating arrays , if we have an element with the same value as another element from the same array, we would expect PHP instead of creating new zval container to increase the refcount and point the duplicate symbol to the same zval. This is true except for value type integer. <br>Example:<br><br>$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; 23, &apos;too&apos; =&gt; 23 ];<br>xdebug_debug_zval( &apos;arr&apos; );<br><br>Output:<br>arr:<br><br>(refcount=2, is_ref=0)<br>array (size=3)<br>  &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)<br>  &apos;age&apos; =&gt; (refcount=0, is_ref=0)int 23<br>  &apos;too&apos; =&gt; (refcount=0, is_ref=0)int 23<br><br>but :<br>$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; 23, &apos;too&apos; =&gt; &apos;23&apos; ];<br>xdebug_debug_zval( &apos;arr&apos; );<br><br>will produce:<br>arr:<br><br>(refcount=2, is_ref=0)<br>array (size=3)<br>  &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)<br>  &apos;age&apos; =&gt; (refcount=0, is_ref=0)int 23<br>  &apos;too&apos; =&gt; (refcount=1, is_ref=0)string &apos;23&apos; (length=2)<br>or :<br><br>$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; [1,2], &apos;too&apos; =&gt; [1,2] ];<br>xdebug_debug_zval( &apos;arr&apos; );<br><br>Output:<br>arr:<br><br>(refcount=2, is_ref=0)<br>array (size=3)<br>  &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)<br>  &apos;age&apos; =&gt; (refcount=2, is_ref=0)<br>    array (size=2)<br>      0 =&gt; (refcount=0, is_ref=0)int 1<br>      1 =&gt; (refcount=0, is_ref=0)int 2<br>  &apos;too&apos; =&gt; (refcount=2, is_ref=0)<br>    array (size=2)<br>      0 =&gt; (refcount=0, is_ref=0)int 1<br>      1 =&gt; (refcount=0, is_ref=0)int 2  

#

[Editor&apos;s note: You can achieve what you&apos;re looking for by referencing $single, rather than copying it by value in your foreach statement. See http://php.net/foreach for more details.]<br><br>Don&apos;t know if this is known or not, but it did eat some of my time and maybe it won&apos;t eat your time now...<br><br>I tried to add something to a multidimensional array, but that didn&apos;t work at first, look at the code below to see what I mean:<br><br>

```
<?php

$a1 = array( "a" => 0, "b" => 1 );
$a2 = array( "aa" => 00, "bb" => 11 );

$together = array( $a1, $a2 );

foreach( $together as $single ) {
    $single[ "c" ] = 3 ;
}

print_r( $together ); 
/* nothing changed result is:
Array
(
    [0] => Array
        (
            [a] => 0
            [b] => 1
        )
    [1] => Array
        (
            [aa] => 0
            [bb] => 11
        )
) */

foreach( $together as $key => $value ) {
    $together[$key]["c"] = 3 ;
}

print_r( $together );

/* now it works, this prints
Array
(
    [0] => Array
        (
            [a] => 0
            [b] => 1
            [c] => 3
        )
    [1] => Array
        (
            [aa] => 0
            [bb] => 11
            [c] => 3
        )
)
*/

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.array.php)

**[To root](/README.md)**