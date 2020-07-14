# str_replace



A faster way to replace the strings in multidimensional array is to json_encode() it, do the str_replace() and then json_decode() it, like this:<br><br>

```
<?php
function str_replace_json($search, $replace, $subject){
     return json_decode(str_replace($search, $replace,  json_encode($subject)));

}
?>
```


This method is almost 3x faster (in 10000 runs.) than using recursive calling and looping method, and 10x simpler in coding.

Compared to:



```
<?php
function str_replace_deep($search, $replace, $subject)
{
    if (is_array($subject))
    {
        foreach($subject as &amp;$oneSubject)
            $oneSubject = str_replace_deep($search, $replace, $oneSubject);
        unset($oneSubject);
        return $subject;
    } else {
        return str_replace($search, $replace, $subject);
    }
}
?>
```
  

#

Note that this does not replace strings that become part of replacement strings. This may be a problem when you want to remove multiple instances of the same repetative pattern, several times in a row.<br><br>If you want to remove all dashes but one from the string &apos;-aaa----b-c-----d--e---f&apos; resulting in &apos;-aaa-b-c-d-e-f&apos;, you cannot use str_replace. Instead, use preg_replace:<br><br>

```
<?php
$challenge = &apos;-aaa----b-c-----d--e---f&apos;;
echo str_replace(&apos;--&apos;, &apos;-&apos;, $challenge).&apos;&lt;br&gt;&apos;;
echo preg_replace(&apos;/--+/&apos;, &apos;-&apos;, $challenge).&apos;&lt;br&gt;&apos;;
?>
```
<br><br>This outputs the following:<br>-aaa--b-c---d-e--f<br>-aaa-b-c-d-e-f  

#

Feel free to optimize this using the while/for or anything else, but this is a bit of code that allows you to replace strings found in an associative array.<br><br>For example:<br>

```
<?php
$replace = array(
&apos;dog&apos; =&gt; &apos;cat&apos;,
&apos;apple&apos; =&gt; &apos;orange&apos;
&apos;chevy&apos; =&gt; &apos;ford&apos;
);

$string = &apos;I like to eat an apple with my dog in my chevy&apos;;

echo str_replace_assoc($replace,$string);

// Echo: I like to eat an orange with my cat in my ford
?>
```


Here is the function:



```
<?php
function strReplaceAssoc(array $replace, $subject) {
   return str_replace(array_keys($replace), array_values($replace), $subject);    
}
?>
```
<br><br>[Jun 1st, 2010 - EDIT BY thiago AT php DOT net: Function has been replaced with an updated version sent by ljelinek AT gmail DOT com]  

#

Be careful when replacing characters (or repeated patterns in the FROM and TO arrays):<br><br>For example:<br><br>

```
<?php
$arrFrom = array("1","2","3","B");
$arrTo = array("A","B","C","D");
$word = "ZBB2";
echo str_replace($arrFrom, $arrTo, $word);
?>
```


I would expect as result: "ZDDB"
However, this return: "ZDDD"
(Because B = D according to our array)

To make this work, use "strtr" instead:



```
<?php
$arr = array("1" =&gt; "A","2" =&gt; "B","3" =&gt; "C","B" =&gt; "D");
$word = "ZBB2";
echo strtr($word,$arr);
?>
```
<br><br>This returns: "ZDDB"  

#

Be aware that if you use this for filtering &amp; sanitizing some form of user input, or remove ALL instances of a string, there&apos;s another gotcha to watch out for:<br><br>// Remove all double characters<br>$string="1001011010";<br>$string=str_replace(array("11","00"),"",$string);<br>// Output: "110010"<br><br>$string="&lt;ht&lt;html&gt;ml&gt; Malicious code &lt;/&lt;html&gt;html&gt; etc";<br>$string=str_replace(array("&lt;html&gt;","&lt;/html&gt;"),"",$string);<br>// Output: "&lt;html&gt; Malicious code &lt;/html&gt; etc"  

#

[Official documentation page](https://www.php.net/manual/en/function.str-replace.php)

**[To root](/README.md)**