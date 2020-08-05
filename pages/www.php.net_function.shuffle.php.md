# shuffle



shuffle for associative arrays, preserves key=&gt;value pairs.<br>(Based on (Vladimir Kornea of typetango.com)&apos;s function) <br><br>

```
<?php
    function shuffle_assoc(&amp;$array) {
        $keys = array_keys($array);

        shuffle($keys);

        foreach($keys as $key) {
            $new[$key] = $array[$key];
        }

        $array = $new;

        return true;
    }
?>
```
<br><br>*note: as of PHP 5.2.10, array_rand&apos;s resulting array of keys is no longer shuffled, so we use array_keys + shuffle.  

---

Shuffle associative and non-associative array while preserving key, value pairs. Also returns the shuffled array instead of shuffling it in place.<br><br>

```
<?php

function shuffle_assoc($list) {
  if (!is_array($list)) return $list;

  $keys = array_keys($list);
  shuffle($keys);
  $random = array();
  foreach ($keys as $key)
    $random[$key] = $list[$key];

  return $random;
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.shuffle.php)

**[To root](/README.md)**