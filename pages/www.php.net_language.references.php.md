# References Explained



Another example of something to watch out for when using references with arrays.  It seems that even an usused reference to an array cell modifies the *source* of the reference.  Strange behavior for an assignment statement (is this why I&apos;ve seen it written as an =&amp; operator?  - although this doesn&apos;t happen with regular variables).<br>

```
<?php
    $array1 = array(1,2);
    $x = &amp;$array1[1];   // Unused reference
    $array2 = $array1;  // reference now also applies to $array2 !
    $array2[1]=22;      // (changing [0] will not affect $array1)
    print_r($array1);
?>
```

Produces:
    Array
    (
    [0] =&gt; 1
    [1] =&gt; 22    // var_dump() will show the &amp; here
    )

I fixed my bug by rewriting the code without references, but it can also be fixed with the unset() function:


```
<?php
    $array1 = array(1,2);
    $x = &amp;$array1[1];
    $array2 = $array1;
    unset($x); // Array copy is now unaffected by above reference
    $array2[1]=22;
    print_r($array1);
?>
```
<br>Produces:<br>    Array<br>    (<br>    [0] =&gt; 1<br>    [1] =&gt; 2<br>    )  

#

On the post that says php4 automagically makes references, this appears to *not* apply to objects:<br><br>http://www.php.net/manual/en/language.references.whatdo.php<br><br>"Note:  Not using the &amp; operator causes a copy of the object to be made. If you use $this in the class it will operate on the current instance of the class. The assignment without &amp; will copy the instance (i.e. the object) and $this will operate on the copy, which is not always what is desired. Usually you want to have a single instance to work with, due to performance and memory consumption issues."  

#

A little gotcha (be careful with references!):<br><br>

```
<?php
$arr = array(&apos;a&apos;=&gt;&apos;first&apos;, &apos;b&apos;=&gt;&apos;second&apos;, &apos;c&apos;=&gt;&apos;third&apos;);
foreach ($arr as &amp;$a); // do nothing. maybe?
foreach ($arr as $a);  // do nothing. maybe?
print_r($arr);
?>
```
<br>Output:<br><br>Array<br>(<br>    [a] =&gt; first<br>    [b] =&gt; second<br>    [c] =&gt; second<br>)<br><br>Add &apos;unset($a)&apos; between the foreachs to obtain the &apos;correct&apos; output:<br><br>Array<br>(<br>    [a] =&gt; first<br>    [b] =&gt; second<br>    [c] =&gt; third<br>)  

#

Here is a good magazine article (PDF format) that explains the internals of PHP&apos;s reference mechanism in detail: http://derickrethans.nl/files/phparch-php-variables-article.pdf<br><br>It should explain some of the odd behavior PHP sometimes seems to exhibit, as well as why you can&apos;t create "references to references" (unlike in C++), and why you should never attempt to use references to speed up passing of large strings or arrays (it will make no difference, or it will slow things down).<br><br>It was written for PHP 4 but it still applies. The only difference is in how PHP 5 handles objects: passing object variables by value only copies an internal pointer to the object. Objects in PHP 5 are only ever duplicated if you explicitly use the clone keyword.  

#

in addition to what &apos;jw at jwscripts dot com&apos; wrote about unset; it can also be used to "detach" the variable alias so that it may work on a unique piece of memory again.<br><br>here&apos;s an example<br><br>

```
<?php
define(&apos;NL&apos;, "\r\n");

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
    $e = &apos;$detach = &apos; . $v . &apos;;&apos;;
    $e .= &apos;unset(&apos;.$v.&apos;);&apos;;
    $e .= $v . &apos; = $detach;&apos;;
    return $e;
}
?>
```
<br><br>output {<br>before:<br>v1=shared<br>v2=shared<br>v3=shared<br>v4=shared<br><br>after:<br>v1=shared?<br>v2=shared no more<br>v3=shared still<br>v4=shared still<br>}<br><br>http://www.obdev.at/developers/articles/00002.html says there&apos;s no such thing as an "object reference" in PHP, but with detaching it becomes possible.<br><br>Hopefully detach, or something like it, will become a language construct in the future.  

#

This is discussed before (below) but bears repeating:<br><br>$a = null; ($a =&amp; null; does not parse) is NOT the same as unset($a);<br><br>$a = null; replaces the value at the destination of $a with the null value;<br><br>If you chose to use a convention like $NULL = NULL;<br><br>THEN, you could say $a =&amp; $NULL to break any previous reference assignment to $a (setting it of course to $NULL), which could still get you into trouble if you forgot and then said $a = &apos;5&apos;. Now $NULL would be &apos;5&apos;.<br><br>Moral: use unset when it is called for.  

#

[Official documentation page](https://www.php.net/manual/en/language.references.php)

**[To root](/README.md)**