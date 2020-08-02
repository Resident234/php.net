# preg_last_error



In PHP 5.5 and above, getting the error message is as simple as:<br><br>

```
<?php
echo array_flip(get_defined_constants(true)['pcre'])[preg_last_error()];?>
```
  

---

Here is a more advanced function to convert an error code to text:<br><br>

```
<?php

function preg_errtxt($errcode)
{
    static $errtext;

    if (!isset($errtxt))
    {
        $errtext = array();
        $constants = get_defined_constants(true);
        foreach ($constants['pcre'] as $c => $n) if (preg_match('/_ERROR$/', $c)) $errtext[$n] = $c;
    }

    return array_key_exists($errcode, $errtext)? $errtext[$errcode] : NULL;
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.preg-last-error.php)

**[To root](/README.md)**