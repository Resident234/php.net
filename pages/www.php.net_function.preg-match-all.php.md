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

---

The code that john at mccarthy dot net posted is not necessary. If you want your results grouped by individual match simply use:<br><br>

```
<?php
preg_match_all($pattern, $string, $matches, PREG_SET_ORDER);
?>
```


E.g.



```
<?php
preg_match_all('/([GH])([12])([!?])/', 'G1? H2!', $matches); // Default PREG_PATTERN_ORDER
// $matches = array(0 => array(0 => 'G1?', 1 => 'H2!'),
//                  1 => array(0 => 'G', 1 => 'H'),
//                  2 => array(0 => '1', 1 => '2'),
//                  3 => array(0 => '?', 1 => '!'))

preg_match_all('/([GH])([12])([!?])/', 'G1? H2!', $matches, PREG_SET_ORDER);
// $matches = array(0 => array(0 => 'G1?', 1 => 'G', 2 => '1', 3 => '?'),
//                  1 => array(0 => 'H2!', 1 => 'H', 2 => '2', 3 => '!'))
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.preg-match-all.php)

**[To root](/README.md)**