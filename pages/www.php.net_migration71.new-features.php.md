# New features





```
<?php
function swap( &amp;$a, &amp;$b ): void
  { [ $a, $b ] = [ $b, $a ]; }
?>
```
  

#

Note that declaring nullable return type does not mean that you can skip return statement at all. For example:<br><br>php &gt; function a(): ?string { }<br>php &gt; a();<br>PHP Warning:  Uncaught TypeError: Return value of a() must be of the type string or null, none returned in php shell code:2<br><br>php &gt; function b(): ?string { return; }<br>PHP Fatal error:  A function with return type must return a value (did you mean "return null;" instead of "return;"?) in php shell code on line 2  

#

[Official documentation page](https://www.php.net/manual/en/migration71.new-features.php)

**[To root](/README.md)**