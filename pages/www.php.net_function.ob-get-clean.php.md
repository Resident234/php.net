# ob_get_clean



The definition should mention that the function also "turns off output buffering", not just cleans it.  

---

Also, don&apos;t forget that you will need to ob_start() again for any successive calls:<br><br>

```
<?php
ob_start();
echo "1";
$content = ob_get_clean();

ob_start(); // This is NECESSARY for the next ob_get_clean() to work as intended.
echo "2";
$content .= ob_get_clean();

echo $content;
?>
```
<br><br>Output: 12<br><br>Without the second ob_start(), the output is 21 ...  

---

[Official documentation page](https://www.php.net/manual/en/function.ob-get-clean.php)

**[To root](/README.md)**