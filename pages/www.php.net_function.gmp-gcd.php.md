# gmp_gcd



here is an elegant recursive solution<br>

```
<?php    

function gcd($a,$b) {
    return ($a % $b) ? gcd($b,$a % $b) : $b;
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.gmp-gcd.php)

**[To root](/README.md)**