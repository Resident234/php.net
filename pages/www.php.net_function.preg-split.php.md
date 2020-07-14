# preg_split



Sometimes PREG_SPLIT_DELIM_CAPTURE does strange results.<br><br>

```
<?php
$content = &apos;&lt;strong&gt;Lorem ipsum dolor&lt;/strong&gt; sit &lt;img src="test.png" /&gt;amet &lt;span class="test" style="color:red"&gt;consec&lt;i&gt;tet&lt;/i&gt;uer&lt;/span&gt;.&apos;;
$chars = preg_split(&apos;/&lt;[^&gt;]*[^\/]&gt;/i&apos;, $content, -1, PREG_SPLIT_NO_EMPTY | PREG_SPLIT_DELIM_CAPTURE);
print_r($chars);
?>
```

Produces:
Array
(
    [0] =&gt; Lorem ipsum dolor
    [1] =&gt;  sit &lt;img src="test.png" /&gt;amet 
    [2] =&gt; consec
    [3] =&gt; tet
    [4] =&gt; uer
)

So that the delimiter patterns are missing. If you wanna get these patters remember to use parentheses.



```
<?php
$chars = preg_split(&apos;/(&lt;[^&gt;]*[^\/]&gt;)/i&apos;, $content, -1, PREG_SPLIT_NO_EMPTY | PREG_SPLIT_DELIM_CAPTURE);
print_r($chars); //parentheses added
?>
```
<br>Produces:<br>Array<br>(<br>    [0] =&gt; &lt;strong&gt;<br>    [1] =&gt; Lorem ipsum dolor<br>    [2] =&gt; &lt;/strong&gt;<br>    [3] =&gt;  sit &lt;img src="test.png" /&gt;amet <br>    [4] =&gt; &lt;span class="test" style="color:red"&gt;<br>    [5] =&gt; consec<br>    [6] =&gt; &lt;i&gt;<br>    [7] =&gt; tet<br>    [8] =&gt; &lt;/i&gt;<br>    [9] =&gt; uer<br>    [10] =&gt; &lt;/span&gt;<br>    [11] =&gt; .<br>)  

#

Extending m.timmermans&apos;s solution, you can use the following code as a search expression parser:<br><br>

```
<?php
$search_expression = "apple bear \"Tom Cruise\" or &apos;Mickey Mouse&apos; another word";
$words = preg_split("/[\s,]*\\\"([^\\\"]+)\\\"[\s,]*|" . "[\s,]*&apos;([^&apos;]+)&apos;[\s,]*|" . "[\s,]+/", $search_expression, 0, PREG_SPLIT_NO_EMPTY | PREG_SPLIT_DELIM_CAPTURE);
print_r($words);
?>
```
<br><br>The result will be:<br>Array<br>(<br>    [0] =&gt; apple<br>    [1] =&gt; bear<br>    [2] =&gt; Tom Cruise<br>    [3] =&gt; or<br>    [4] =&gt; Mickey Mouse<br>    [5] =&gt; another<br>    [6] =&gt; word<br>)<br><br>1. Accepted delimiters: white spaces (space, tab, new line etc.) and commas.<br><br>2. You can use either simple (&apos;) or double (") quotes for expressions which contains more than one word.  

#

If you want to split by a char, but want to ignore that char in case it is escaped, use a lookbehind assertion.<br><br>In this example a string will be split by ":" but "\:" will be ignored:<br><br>

```
<?php
$string=&apos;a:b:c\:d&apos;;
$array=preg_split(&apos;#(?&lt;!\\\)\:#&apos;,$string);
print_r($array);
?>
```
<br><br>Results into:<br><br>Array<br>(<br>    [0] =&gt; a<br>    [1] =&gt; b<br>    [2] =&gt; c\:d<br>)  

#

Here is another way to split a CamelCase string, which is a simpler expression than the one using lookaheads and lookbehinds: <br><br>preg_split(&apos;/([[:upper:]][[:lower:]]+)/&apos;, $last, null, PREG_SPLIT_DELIM_CAPTURE|PREG_SPLIT_NO_EMPTY)<br><br>It makes the entire CamelCased word the delimiter, then returns the delimiters (PREG_SPLIT_DELIM_CAPTURE) and omits the empty values between the delimiters (PREG_SPLIT_NO_EMPTY)  

#

To clarify the "limit" parameter and the PREG_SPLIT_DELIM_CAPTURE option,<br><br>

```
<?php
$preg_split(&apos;(/ /)&apos;, &apos;1 2 3 4 5 6 7 8&apos;, 4 ,PREG_SPLIT_DELIM_CAPTURE );
?>
```
<br><br>returns:<br><br>(&apos;1&apos;, &apos; &apos;, &apos;2&apos;, &apos; &apos; , &apos;3&apos;, &apos; &apos;, &apos;4 5 6 7 8&apos;)<br><br>So you actually get 7 array items not 4  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-split.php)

**[To root](/README.md)**