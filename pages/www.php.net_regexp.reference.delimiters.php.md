# Delimiters



Note that bracket style opening and closing delimiters aren&apos;t a 100% problem-free solution, as they need to be escaped when they aren&apos;t in matching pairs within the expression. That mismatch can happen when they appear inside character classes [...], as most meta-characters lose their special meaning. Consider these examples:<br><br>

```
<?php
  preg_match('{[{]}', ''); // Warning: preg_match(): No ending matching delimiter '}'
  preg_match('{[}]}', ''); // Warning: preg_match(): Unknown modifier ']'
  preg_match('{[}{]}', ''); // Warning: preg_match(): Unknown modifier ']'
?>
```


Escaping them solves it:



```
<?php
  preg_match('{[\{]}', ''); // OK
  preg_match('{[}]}', ''); // OK
  preg_match('{[\}\{]}', ''); // OK
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/regexp.reference.delimiters.php)

**[To root](/README.md)**