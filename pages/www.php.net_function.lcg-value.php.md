# lcg_value



An elegant way to return random float between two numbers:<br><br>

```
<?php
function random_float ($min,$max) {
   return ($min+lcg_value()*(abs($max-$min)));
}
?>
```
  

---

Choose your weapon:<br>

```
<?php
function mt_randf($min, $max)
{
    return $min + abs($max - $min) * mt_rand(0, mt_getrandmax())/mt_getrandmax(); 
}
function lcg_randf($min, $max)
{
    return $min + lcg_value() * abs($max - $min);
}
function randf($min, $max)
{
    return $min + rand(0,getrandmax()) / getrandmax() * abs($max - $min);
}?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.lcg-value.php)

**[To root](/README.md)**