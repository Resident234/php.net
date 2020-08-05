# mb_substr



Passing null as length will not make mb_substr use it&apos;s default, instead it will interpret it as 0.<br>

```
<?php
mb_substr($str,$start,null,$encoding); //Returns '' (empty string) just like substr()
?>
```

Instead use:


```
<?php
mb_substr($str,$start,mb_strlen($str),$encoding);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.mb-substr.php)

**[To root](/README.md)**