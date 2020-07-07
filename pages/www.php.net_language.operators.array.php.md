# Array Operators





The union operator did not behave as I thought it would on first glance. It implements a union (of sorts) based on the keys of the array, not on the values.

For instance:


```
<?php
$a = array(&apos;one&apos;,&apos;two&apos;);
$b=array(&apos;three&apos;,&apos;four&apos;,&apos;five&apos;);

//not a union of arrays&apos; values
echo &apos;$a + $b : &apos;;
print_r ($a + $b);

//a union of arrays&apos; values
echo &quot;array_unique(array_merge($a,$b)):&quot;;
// cribbed from http://oreilly.com/catalog/progphp/chapter/ch05.html
print_r (array_unique(array_merge($a,$b)));
?>
```


//output

$a + $b : Array
(
&#xA0; &#xA0; [0] =&gt; one
&#xA0; &#xA0; [1] =&gt; two
&#xA0; &#xA0; [2] =&gt; five
)
array_unique(array_merge(Array,Array)):Array
(
&#xA0; &#xA0; [0] =&gt; one
&#xA0; &#xA0; [1] =&gt; two
&#xA0; &#xA0; [2] =&gt; three
&#xA0; &#xA0; [3] =&gt; four
&#xA0; &#xA0; [4] =&gt; five
)

  

#



The example may get u into thinking that the identical operator returns true because the key of apple is a string but that is not the case, cause if a string array key is the standart representation of a integer it&apos;s gets a numeral key automaticly. 

The identical operator just requires that the keys are in the same order in both arrays:



```
<?php
$a = array (0 =&gt; &quot;apple&quot;, 1 =&gt; &quot;banana&quot;);
$b = array (1 =&gt; &quot;banana&quot;, 0 =&gt; &quot;apple&quot;);

var_dump($a === $b); // prints bool(false) as well

$b = array (&quot;0&quot; =&gt; &quot;apple&quot;, &quot;1&quot; =&gt; &quot;banana&quot;);

var_dump($a === $b); // prints bool(true)
?>
```



  

#



It should be mentioned that the array union operator functions almost identically to array_replace with the exception that precedence of arguments is reversed.

  

#



Note that + will not renumber numeric array keys.&#xA0; If you have two numeric arrays, and their indices overlap, + will use the first array&apos;s values for each numeric key, adding the 2nd array&apos;s values only where the first doesn&apos;t already have a value for that index.&#xA0; Example:

$a = array(&apos;red&apos;, &apos;orange&apos;);
$b = array(&apos;yellow&apos;, &apos;green&apos;, &apos;blue&apos;);
$both = $a + $b;
var_dump($both);

Produces the output:

array(3) { [0]=&gt;&#xA0; string(3) &quot;red&quot; [1]=&gt;&#xA0; string(6) &quot;orange&quot; [2]=&gt;&#xA0; string(4) &quot;blue&quot; }

To get a 5-element array, use array_merge.

&#xA0; &#xA0; Dan

  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.array.php)

**[To root](/README.md)**