# strrev



This function support utf-8 encoding, Human Language and Character Encoding Support:<br><br>

```
<?php
function mb_strrev($str){
    $r = '';
    for ($i = mb_strlen($str); $i>=0; $i--) {
        $r .= mb_substr($str, $i, 1);
    }
    return $r;
}

echo mb_strrev("&#x2606;&#x2764;world"); // echo "dlrow&#x2764;&#x2606;"
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.strrev.php)

**[To root](/README.md)**