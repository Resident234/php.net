# preg_match_all



if you want to extract all {token}s from a string:<br><br>

```
<?php
$pattern = "/{[^}]*}/";
$subject = "{token1} foo {token2} bar";
preg_match_all($pattern, $subject, $matches);
print_r($matches);
?>
```
<br><br>output:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [0] =&gt; {token1}<br>            [1] =&gt; {token2}<br>        )<br><br>)  

#

The code that john at mccarthy dot net posted is not necessary. If you want your results grouped by individual match simply use:<br><br>&lt;?<br>preg_match_all($pattern, $string, $matches, PREG_SET_ORDER);<br>?>
```
<br><br>E.g.<br><br>&lt;?<br>preg_match_all(&apos;/([GH])([12])([!?])/&apos;, &apos;G1? H2!&apos;, $matches); // Default PREG_PATTERN_ORDER<br>// $matches = array(0 =&gt; array(0 =&gt; &apos;G1?&apos;, 1 =&gt; &apos;H2!&apos;),<br>//                  1 =&gt; array(0 =&gt; &apos;G&apos;, 1 =&gt; &apos;H&apos;),<br>//                  2 =&gt; array(0 =&gt; &apos;1&apos;, 1 =&gt; &apos;2&apos;),<br>//                  3 =&gt; array(0 =&gt; &apos;?&apos;, 1 =&gt; &apos;!&apos;))<br><br>preg_match_all(&apos;/([GH])([12])([!?])/&apos;, &apos;G1? H2!&apos;, $matches, PREG_SET_ORDER);<br>// $matches = array(0 =&gt; array(0 =&gt; &apos;G1?&apos;, 1 =&gt; &apos;G&apos;, 2 =&gt; &apos;1&apos;, 3 =&gt; &apos;?&apos;),<br>//                  1 =&gt; array(0 =&gt; &apos;H2!&apos;, 1 =&gt; &apos;H&apos;, 2 =&gt; &apos;2&apos;, 3 =&gt; &apos;!&apos;))<br>?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-match-all.php)

**[To root](/README.md)**