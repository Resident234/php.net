# preg_grep



A shorter way to run a match on the array&apos;s keys rather than the values:<br><br>

```
<?php
function preg_grep_keys($pattern, $input, $flags = 0) {
    return array_intersect_key($input, array_flip(preg_grep($pattern, array_keys($input), $flags)));
}
?>
```
  

---

Run a match on the array&apos;s keys rather than the values:<br><br>

```
<?php

function preg_grep_keys( $pattern, $input, $flags = 0 )
{
    $keys = preg_grep( $pattern, array_keys( $input ), $flags );
    $vals = array();
    foreach ( $keys as $key )
    {
        $vals[$key] = $input[$key];
    }
    return $vals;
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.preg-grep.php)

**[To root](/README.md)**