# stripslashes



Sometimes for some reason is happens that PHP or Javascript or some naughty insert a lot of  backslash. Ordinary function does not notice that. Therefore, it is necessary that the bit "inflate":<br><br>

```
<?php
function removeslashes($string)
{
    $string=implode("",explode("\\",$string));
    return stripslashes(trim($string));
}

/* Example */

$text="My dog don\\\\\\\\\\\\\\\\'t like the postman!";
echo removeslashes($text);
?>
```
<br><br>RESULT: My dog don&apos;t like the postman!<br><br>This flick has served me wery well, because I had this problem before.  

---

[Official documentation page](https://www.php.net/manual/en/function.stripslashes.php)

**[To root](/README.md)**