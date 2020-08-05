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
    $l = mb_strlen($input, 'UTF-8');
    $unique = array();
    for($i = 0; $i < $l; $i++) {
        $char = mb_substr($input, $i, 1, 'UTF-8');
        if(!array_key_exists($char, $unique))
            $unique[$char] = 0;
        $unique[$char]++;
    }
    return $unique;
}

$input = "Let's try some Greek letters: &#x3B1;&#x3B1;&#x3B1;&#x3B1;&#x3B1;&#x395;&#x3B5;&#x399;&#x3B9;&#x39C;&#x3BC;&#x3A8;&#x3C8;, Russian: &#x419;&#x419;&#x42B;&#x42B;&#x429;&#x41D;, Czech: &#x11B;&#x161;&#x10D;&#x159;&#x17E;&#xFD;&#xE1;&#xED;&#xE9;";
print_r( mb_count_chars($input) ); 
//returns: Array ( [L] => 1 [e] => 7 [t] => 4 ['] => 1 [s] => 5 [ ] => 9 [r] => 3 [y] => 1 [o] => 1 [m] => 1 [G] => 1 [k] => 1 [l] => 1 [:] => 3 [&#x3B1;] => 5 [&#x395;] => 1 [&#x3B5;] => 1 [&#x399;] => 1 [&#x3B9;] => 1 [&#x39C;] => 1 [&#x3BC;] => 1 [&#x3A8;] => 1 [&#x3C8;] => 1 [,] => 2 [R] => 1 [u] => 1 [i] => 1 [a] => 1 [n] => 1 [&#x419;] => 2 [&#x42B;] => 2 [&#x429;] => 1 [&#x41D;] => 1 [C] => 1 [z] => 1 [c] => 1 [h] => 1 [&#x11B;] => 1 [&#x161;] => 1 [&#x10D;] => 1 [&#x159;] => 1 [&#x17E;] => 1 [&#xFD;] => 1 [&#xE1;] => 1 [&#xED;] => 1 [&#xE9;] => 1 )
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.count-chars.php)

**[To root](/README.md)**