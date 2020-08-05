# eval



Kepp the following Quote in mind:<br><br>If eval() is the answer, you&apos;re almost certainly asking the<br>wrong question. -- Rasmus Lerdorf, BDFL of PHP  

---

Inception with eval()<br><br><br>Inception Start:<br>

```
<?php
eval("echo 'Inception lvl 1...\n'; eval('echo \"Inception lvl 2...\n\"; eval(\"echo \'Inception lvl 3...\n\'; eval(\'echo \\\"Limbo!\\\";\');\");');");
?>
```
  

---

At least in PHP 7.1+, eval() terminates the script if the evaluated code generate a fatal error. For example:<br>

```
<?php
@eval('$content = (100 - );');
?>
```


(Even if it is in the man, I'm note sure it acted like this in 5.6, but whatever)
To catch it, I had to do:


```
<?php
try {
    eval('$content = (100 - );');
} catch (Throwable $t) {
    $content = null;
}
?>
```
<br><br>This is the only way I found to catch the error and hide the fact there was one.  

---

If you want to allow math input and make sure that the input is proper mathematics and not some hacking code, you can try this:<br><br>

```
<?php

$test = '2+3*pi';

// Remove whitespaces
$test = preg_replace('/\s+/', '', $test);

$number = '(?:\d+(?:[,.]\d+)?|pi|&#x3C0;)'; // What is a number
$functions = '(?:sinh?|cosh?|tanh?|abs|acosh?|asinh?|atanh?|exp|log10|deg2rad|rad2deg|sqrt|ceil|floor|round)'; // Allowed PHP functions
$operators = '[+\/*\^%-]'; // Allowed math operators
$regexp = '/^(('.$number.'|'.$functions.'\s*\((?1)+\)|\((?1)+\))(?:'.$operators.'(?2))?)+$/'; // Final regexp, heavily using recursive patterns

if (preg_match($regexp, $q))
{
    $test = preg_replace('!pi|&#x3C0;!', 'pi()', $test); // Replace pi with pi function
    eval('$result = '.$test.';');
}
else
{
    $result = false;
}

?>
```
<br><br>I can&apos;t guarantee you absolutely that this will block every possible malicious code nor that it will block malformed code, but that&apos;s better than the matheval function below which will allow malformed code like &apos;2+2+&apos; which will throw an error.  

---

[Official documentation page](https://www.php.net/manual/en/function.eval.php)

**[To root](/README.md)**