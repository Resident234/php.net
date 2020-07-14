# count_chars



If you have problems using count_chars with a multibyte string, you can change the page encoding. Alternatively, you can also use this mb_count_chars version of the function. Basically it is mode "1" of the original function.<br><br>

```
<?php
/**
 * Counts character occurences in a multibyte string
 * @param string $input UTF-8 data
 * @return array associative array of characters.
 */
function mb_count_chars($input) {
    $l = mb_strlen($input, &apos;UTF-8&apos;);
    $unique = array();
    for($i = 0; $i &lt; $l; $i++) {
        $char = mb_substr($input, $i, 1, &apos;UTF-8&apos;);
        if(!array_key_exists($char, $unique))
            $unique[$char] = 0;
        $unique[$char]++;
    }
    return $unique;
}

$input = "Let&apos;s try some Greek letters: &#x3B1;&#x3B1;&#x3B1;&#x3B1;&#x3B1;&#x395;&#x3B5;&#x399;&#x3B9;&#x39C;&#x3BC;&#x3A8;&#x3C8;, Russian: &#x419;&#x419;&#x42B;&#x42B;&#x429;&#x41D;, Czech: &#x11B;&#x161;&#x10D;&#x159;&#x17E;&#xFD;&#xE1;&#xED;&#xE9;";
print_r( mb_count_chars($input) ); 
//returns: Array ( [L] =&gt; 1 [e] =&gt; 7 [t] =&gt; 4 [&apos;] =&gt; 1 [s] =&gt; 5 [ ] =&gt; 9 [r] =&gt; 3 [y] =&gt; 1 [o] =&gt; 1 [m] =&gt; 1 [G] =&gt; 1 [k] =&gt; 1 [l] =&gt; 1 [:] =&gt; 3 [&#x3B1;] =&gt; 5 [&#x395;] =&gt; 1 [&#x3B5;] =&gt; 1 [&#x399;] =&gt; 1 [&#x3B9;] =&gt; 1 [&#x39C;] =&gt; 1 [&#x3BC;] =&gt; 1 [&#x3A8;] =&gt; 1 [&#x3C8;] =&gt; 1 [,] =&gt; 2 [R] =&gt; 1 [u] =&gt; 1 [i] =&gt; 1 [a] =&gt; 1 [n] =&gt; 1 [&#x419;] =&gt; 2 [&#x42B;] =&gt; 2 [&#x429;] =&gt; 1 [&#x41D;] =&gt; 1 [C] =&gt; 1 [z] =&gt; 1 [c] =&gt; 1 [h] =&gt; 1 [&#x11B;] =&gt; 1 [&#x161;] =&gt; 1 [&#x10D;] =&gt; 1 [&#x159;] =&gt; 1 [&#x17E;] =&gt; 1 [&#xFD;] =&gt; 1 [&#xE1;] =&gt; 1 [&#xED;] =&gt; 1 [&#xE9;] =&gt; 1 )
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.count-chars.php)

**[To root](/README.md)**