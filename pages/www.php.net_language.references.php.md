# References Explained





Another example of something to watch out for when using references with arrays.&#xA0; It seems that even an usused reference to an array cell modifies the *source* of the reference.&#xA0; Strange behavior for an assignment statement (is this why I&apos;ve seen it written as an =&amp; operator?&#xA0; - although this doesn&apos;t happen with regular variables).


```
<?php
&#xA0; &#xA0; $array1 = array(1,2);
&#xA0; &#xA0; $x = &amp;$array1[1];&#xA0;&#xA0; // Unused reference
&#xA0; &#xA0; $array2 = $array1;&#xA0; // reference now also applies to $array2 !
&#xA0; &#xA0; $array2[1]=22;&#xA0; &#xA0; &#xA0; // (changing [0] will not affect $array1)
&#xA0; &#xA0; print_r($array1);
?>
```

Produces:
&#xA0; &#xA0; Array
&#xA0; &#xA0; (
&#xA0; &#xA0; [0] =&gt; 1
&#xA0; &#xA0; [1] =&gt; 22&#xA0; &#xA0; // var_dump() will show the &amp; here
&#xA0; &#xA0; )

I fixed my bug by rewriting the code without references, but it can also be fixed with the unset() function:


```
<?php
&#xA0; &#xA0; $array1 = array(1,2);
&#xA0; &#xA0; $x = &amp;$array1[1];
&#xA0; &#xA0; $array2 = $array1;
&#xA0; &#xA0; unset($x); // Array copy is now unaffected by above reference
&#xA0; &#xA0; $array2[1]=22;
&#xA0; &#xA0; print_r($array1);
?>
```

Produces:
&#xA0; &#xA0; Array
&#xA0; &#xA0; (
&#xA0; &#xA0; [0] =&gt; 1
&#xA0; &#xA0; [1] =&gt; 2
&#xA0; &#xA0; )

  

#



On the post that says php4 automagically makes references, this appears to *not* apply to objects:

http://www.php.net/manual/en/language.references.whatdo.php

&quot;Note:&#xA0; Not using the &amp; operator causes a copy of the object to be made. If you use $this in the class it will operate on the current instance of the class. The assignment without &amp; will copy the instance (i.e. the object) and $this will operate on the copy, which is not always what is desired. Usually you want to have a single instance to work with, due to performance and memory consumption issues.&quot;

  

#



A little gotcha (be careful with references!):



```
<?php
$arr = array(&apos;a&apos;=&gt;&apos;first&apos;, &apos;b&apos;=&gt;&apos;second&apos;, &apos;c&apos;=&gt;&apos;third&apos;);
foreach ($arr as &amp;$a); // do nothing. maybe?
foreach ($arr as $a);&#xA0; // do nothing. maybe?
print_r($arr);
?>
```

Output:

Array
(
&#xA0; &#xA0; [a] =&gt; first
&#xA0; &#xA0; [b] =&gt; second
&#xA0; &#xA0; [c] =&gt; second
)

Add &apos;unset($a)&apos; between the foreachs to obtain the &apos;correct&apos; output:

Array
(
&#xA0; &#xA0; [a] =&gt; first
&#xA0; &#xA0; [b] =&gt; second
&#xA0; &#xA0; [c] =&gt; third
)

  

#



Here is a good magazine article (PDF format) that explains the internals of PHP&apos;s reference mechanism in detail: http://derickrethans.nl/files/phparch-php-variables-article.pdf

It should explain some of the odd behavior PHP sometimes seems to exhibit, as well as why you can&apos;t create &quot;references to references&quot; (unlike in C++), and why you should never attempt to use references to speed up passing of large strings or arrays (it will make no difference, or it will slow things down).

It was written for PHP 4 but it still applies. The only difference is in how PHP 5 handles objects: passing object variables by value only copies an internal pointer to the object. Objects in PHP 5 are only ever duplicated if you explicitly use the clone keyword.

  

#



in addition to what &apos;jw at jwscripts dot com&apos; wrote about unset; it can also be used to &quot;detach&quot; the variable alias so that it may work on a unique piece of memory again.

here&apos;s an example



```
<?php
define(&apos;NL&apos;, &quot;\r\n&quot;);

$v1 = &apos;shared&apos;;
$v2 = &amp;$v1;
$v3 = &amp;$v2;
$v4 = &amp;$v3;

echo &apos;before:&apos;.NL;
echo &apos;v1=&apos; . $v1 . NL;
echo &apos;v2=&apos; . $v2 . NL;
echo &apos;v3=&apos; . $v3 . NL;
echo &apos;v4=&apos; . $v4 . NL;

// detach messy
$detach = $v1;
unset($v1);
$v1 = $detach;

// detach pretty, but slower
eval(detach(&apos;$v2&apos;));

$v1 .= &apos;?&apos;;
$v2 .= &apos; no more&apos;;
$v3 .= &apos; sti&apos;;
$v4 .= &apos;ll&apos;;

echo NL.&apos;after:&apos;.NL;
echo &apos;v1=&apos; . $v1 . NL;
echo &apos;v2=&apos; . $v2 . NL;
echo &apos;v3=&apos; . $v3 . NL;
echo &apos;v4=&apos; . $v4 . NL;

function detach($v) {
&#xA0; &#xA0; $e = &apos;$detach = &apos; . $v . &apos;;&apos;;
&#xA0; &#xA0; $e .= &apos;unset(&apos;.$v.&apos;);&apos;;
&#xA0; &#xA0; $e .= $v . &apos; = $detach;&apos;;
&#xA0; &#xA0; return $e;
}
?>
```


output {
before:
v1=shared
v2=shared
v3=shared
v4=shared

after:
v1=shared?
v2=shared no more
v3=shared still
v4=shared still
}

http://www.obdev.at/developers/articles/00002.html says there&apos;s no such thing as an &quot;object reference&quot; in PHP, but with detaching it becomes possible.

Hopefully detach, or something like it, will become a language construct in the future.

  

#



This is discussed before (below) but bears repeating:

$a = null; ($a =&amp; null; does not parse) is NOT the same as unset($a);

$a = null; replaces the value at the destination of $a with the null value;

If you chose to use a convention like $NULL = NULL;

THEN, you could say $a =&amp; $NULL to break any previous reference assignment to $a (setting it of course to $NULL), which could still get you into trouble if you forgot and then said $a = &apos;5&apos;. Now $NULL would be &apos;5&apos;.

Moral: use unset when it is called for.

  

#

[Official documentation page](https://www.php.net/manual/en/language.references.php)

**[To root](/README.md)**