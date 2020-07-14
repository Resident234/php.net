# str_word_count





```
<?php

/***
 * This simple utf-8 word count function (it only counts) 
 * is a bit faster then the one with preg_match_all
 * about 10x slower then the built-in str_word_count
 * 
 * If you need the hyphen or other code points as word-characters
 * just put them into the [brackets] like [^\p{L}\p{N}\&apos;\-]
 * If the pattern contains utf-8, utf8_encode() the pattern,
 * as it is expected to be valid utf-8 (using the u modifier).
 **/

// Jonny 5&apos;s simple word splitter
function str_word_count_utf8($str) {
  return count(preg_split(&apos;~[^\p{L}\p{N}\&apos;]+~u&apos;,$str));
}
?>
```
  

#

We can also specify a range of values for charlist.<br><br>

```
<?php
$str = "Hello fri3nd, you&apos;re
       looking          good today! 
       look1234ing";
print_r(str_word_count($str, 1, &apos;0..3&apos;));
?>
```
<br><br>will give the result as <br><br>Array ( [0] =&gt; Hello [1] =&gt; fri3nd [2] =&gt; you&apos;re [3] =&gt; looking [4] =&gt; good [5] =&gt; today [6] =&gt; look123 [7] =&gt; ing )  

#

[Official documentation page](https://www.php.net/manual/en/function.str-word-count.php)

**[To root](/README.md)**