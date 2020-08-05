# phpversion



If you&apos;re trying to check whether the version of PHP you&apos;re running on is sufficient, don&apos;t screw around with `strcasecmp` etc.  PHP already has a `version_compare` function, and it&apos;s specifically made to compare PHP-style version strings.<br><br>

```
<?php
if (version_compare(phpversion(), '5.3.10', '<')) {
    // php version isn't high enough
}
?>
```
  

---

Note that the version string returned by phpversion() may include more information than expected: "5.5.9-1ubuntu4.17", for example.  

---

To know, what are the {php} extensions loaded &amp; version of extensions :<br><br>

```
<?php
foreach (get_loaded_extensions() as $i => $ext)
{
   echo $ext .' => '. phpversion($ext). '<br/>';
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.phpversion.php)

**[To root](/README.md)**