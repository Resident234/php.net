# mb_split





a (simpler) way to extract all characters from a UTF-8 string to array with a single call to a built-in function:



```
<?php
&#xA0; $str = &apos;&#x41C;&#x430;-
&#x440;&#x443;&#x441;&#x44F;&apos;;
&#xA0; print_r(preg_split(&apos;//u&apos;, $str, null, PREG_SPLIT_NO_EMPTY));
?>
```


Output:

Array
(
&#xA0; &#xA0; [0] =&gt; &#x41C;
&#xA0; &#xA0; [1] =&gt; &#x430;
&#xA0; &#xA0; [2] =&gt; -
&#xA0; &#xA0; [3] =&gt; 

&#xA0; &#xA0; [4] =&gt; &#x440;
&#xA0; &#xA0; [5] =&gt; &#x443;
&#xA0; &#xA0; [6] =&gt; &#x441;
&#xA0; &#xA0; [7] =&gt; &#x44F;
)

  

#



The $pattern argument doesn&apos;t use /pattern/ delimiters, unlike other regex functions such as preg_match.



```
<?php
&#xA0;&#xA0; # Works. No slashes around the /pattern/
&#xA0;&#xA0; print_r( mb_split(&quot;\s&quot;, &quot;hello world&quot;) );
&#xA0;&#xA0; Array (
&#xA0; &#xA0; &#xA0; [0] =&gt; hello
&#xA0; &#xA0; &#xA0; [1] =&gt; world
&#xA0;&#xA0; )

&#xA0;&#xA0; # Doesn&apos;t work:
&#xA0;&#xA0; print_r( mb_split(&quot;/\s/&quot;, &quot;hello world&quot;) );
&#xA0;&#xA0; Array (
&#xA0; &#xA0; &#xA0; [0] =&gt; hello world
&#xA0;&#xA0; )
?>
```



  

#



I figure most people will want a simple way to break-up a multibyte string into its individual characters. Here&apos;s a function I&apos;m using to do that. Change UTF-8 to your chosen encoding method.





```
<?php

function mbStringToArray ($string) {

&#xA0; &#xA0; $strlen = mb_strlen($string);

&#xA0; &#xA0; while ($strlen) {

&#xA0; &#xA0; &#xA0; &#xA0; $array[] = mb_substr($string,0,1,&quot;UTF-8&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; $string = mb_substr($string,1,$strlen,&quot;UTF-8&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; $strlen = mb_strlen($string);

&#xA0; &#xA0; }

&#xA0; &#xA0; return $array;

}

?>
```



  

#



In addition to Sezer Yalcin&apos;s tip.



This function splits a multibyte string into an array of characters. Comparable to str_split().





```
<?php

function mb_str_split( $string ) {

&#xA0; &#xA0; # Split at all position not after the start: ^

&#xA0; &#xA0; # and not before the end: $

&#xA0; &#xA0; return preg_split(&apos;/(?&lt;!^)(?!$)/u&apos;, $string );

}



$string&#xA0;&#xA0; = &apos;&#x706B;&#x8F66;&#x7968;&apos;;

$charlist = mb_str_split( $string );



print_r( $charlist );

?>
```




# Prints:

Array

(

&#xA0; &#xA0; [0] =&gt; &#x706B;

&#xA0; &#xA0; [1] =&gt; &#x8F66;

&#xA0; &#xA0; [2] =&gt; &#x7968;

)

  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-split.php)

**[To root](/README.md)**