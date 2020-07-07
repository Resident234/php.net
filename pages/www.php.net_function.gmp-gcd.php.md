# gmp_gcd





here is an elegant recursive solution


```
<?php&#xA0; &#xA0; 

function gcd($a,$b) {
&#xA0; &#xA0; return ($a % $b) ? gcd($b,$a % $b) : $b;
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.gmp-gcd.php)

**[To root](/README.md)**