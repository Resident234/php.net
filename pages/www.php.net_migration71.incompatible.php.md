# Backward incompatible changes



For anyone migrating from 5.x to 7.1:<br><br>About "Array ordering when elements are automatically created during by reference assignments has changed" on this page<br><br>(http://php.net/manual/en/migration71.incompatible.php#migration71.incompatible.array-order)<br><br>The behaviour of 7.1 is THE SAME as of PHP 5. It is only 7.0 that differs.<br><br>See https://3v4l.org/frbUc<br><br>

```
<?php

$array = [];
$array["a"] =&amp; $array["b"];
$array["b"] = 1;
var_dump($array);?>
```
  

---

The backwards incompatible change &apos;The empty index operator is not supported for strings anymore&apos; has a lot more implications than just a fatal error on the following code<br><br>

```
<?php
$a = "";
$a[] = "hello world";
var_dump($a);
?>
```


This will give a fatal error in 7.1 but will work as expected in 7.0 or below and give you: (no notice, no warning)

array(1) {
  [0]=>
  string(11) "hello world"
}

However, the following is also changed:



```
<?php
$a = "";
$a[0] = "hello world";
var_dump($a);
// 7.1: string(1) "h"
// pre-7.1: array(1) {  [0]=>  string(11) "hello world" }

$a = "";
$a[5] = "hello world";
var_dump($a);
// 7.1: string(6) "     h"
// pre-7.1: array(1) {  [0]=>  string(11) "hello world" }

?>
```
  

---

"OMFG! Why was session.hash_function removed?!? Dude!"<br><br>https://wiki.php.net/rfc/session-id-without-hashing<br><br>There. Saved ya a search.  

---

[Official documentation page](https://www.php.net/manual/en/migration71.incompatible.php)

**[To root](/README.md)**