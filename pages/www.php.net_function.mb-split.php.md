# mb_split



a (simpler) way to extract all characters from a UTF-8 string to array with a single call to a built-in function:<br><br>

```
<?php
  $str = '&#x41C;&#x430;-
&#x440;&#x443;&#x441;&#x44F;';
  print_r(preg_split('//u', $str, null, PREG_SPLIT_NO_EMPTY));
?>
```
<br><br>Output:<br><br>Array<br>(<br>    [0] =&gt; &#x41C;<br>    [1] =&gt; &#x430;<br>    [2] =&gt; -<br>    [3] =&gt; <br><br>    [4] =&gt; &#x440;<br>    [5] =&gt; &#x443;<br>    [6] =&gt; &#x441;<br>    [7] =&gt; &#x44F;<br>)  

#

The $pattern argument doesn&apos;t use /pattern/ delimiters, unlike other regex functions such as preg_match.<br><br>

```
<?php
   # Works. No slashes around the /pattern/
   print_r( mb_split("\s", "hello world") );
   Array (
      [0] => hello
      [1] => world
   )

   # Doesn't work:
   print_r( mb_split("/\s/", "hello world") );
   Array (
      [0] => hello world
   )
?>
```
  

#

I figure most people will want a simple way to break-up a multibyte string into its individual characters. Here&apos;s a function I&apos;m using to do that. Change UTF-8 to your chosen encoding method.<br><br>

```
<?php
function mbStringToArray ($string) {
    $strlen = mb_strlen($string);
    while ($strlen) {
        $array[] = mb_substr($string,0,1,"UTF-8");
        $string = mb_substr($string,1,$strlen,"UTF-8");
        $strlen = mb_strlen($string);
    }
    return $array;
}
?>
```
  

#

In addition to Sezer Yalcin&apos;s tip.<br><br>This function splits a multibyte string into an array of characters. Comparable to str_split().<br><br>

```
<?php
function mb_str_split( $string ) {
    # Split at all position not after the start: ^
    # and not before the end: $
    return preg_split('/(?<!^)(?!$)/u', $string );
}

$string   = '&#x706B;&#x8F66;&#x7968;';
$charlist = mb_str_split( $string );

print_r( $charlist );
?>
```
<br><br># Prints:<br>Array<br>(<br>    [0] =&gt; &#x706B;<br>    [1] =&gt; &#x8F66;<br>    [2] =&gt; &#x7968;<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-split.php)

**[To root](/README.md)**