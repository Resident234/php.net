# is_scalar



Having hunted around the manual, I&apos;ve not found a clear statement of what makes a type "scalar" (e.g. if some future version of the language introduces a new kind of type, what criterion will decide if it&apos;s "scalar"? - that goes beyond just listing what&apos;s scalar in the current version.)<br><br>In other lanuages, it means "has ordering operators" - i.e. "less than" and friends.<br><br>It (-:currently:-) appears to have the same meaning in PHP.  

#

Another warning in response to the previous note:<br>&gt; just a warning as it appears that an empty value is not a scalar.<br><br>That statement is wrong--or, at least, has been fixed with a later revision than the one tested.  The following code generated the following output on PHP 4.3.9.<br><br>CODE:<br>

```
<?php
    echo(&apos;is_scalar() test:&apos;.EOL);
    echo("NULL: "      . print_R(is_scalar(NULL),     true) . EOL);
    echo("false: "    . print_R(is_scalar(false),   true) . EOL);
    echo("(empty): "  . print_R(is_scalar(&apos;&apos;),      true) . EOL);
    echo("0: "         . print_R(is_scalar(0),       true) . EOL);
    echo("&apos;0&apos;: "      . print_R(is_scalar(&apos;0&apos;),     true) . EOL);
?>
```
<br><br>OUTPUT:<br>is_scalar() test:<br>NULL: <br>false: 1<br>(empty): 1<br>0: 1<br>&apos;0&apos;: 1<br><br>THUS:<br>   * NULL is NOT a scalar<br>   * false, (empty string), 0, and "0" ARE scalars  

#

[Official documentation page](https://www.php.net/manual/en/function.is-scalar.php)

**[To root](/README.md)**