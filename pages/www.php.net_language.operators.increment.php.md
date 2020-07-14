# Incrementing/Decrementing Operators



Note that <br><br>$a="9D9"; var_dump(++$a);   =&gt; string(3) "9E0"<br><br>but counting onwards from there <br><br>$a="9E0"; var_dump(++$a);   =&gt; float(10)<br><br>this is due to "9E0" being interpreted as a string representation of the float constant 9E0 (or 9e0), and thus evalutes to 9 * 10^0 = 9 (in a float context)  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.increment.php)

**[To root](/README.md)**