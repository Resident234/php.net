# Superglobals



Since PHP 5.4, you cannot use a superglobal as the parameter to a function. This causes a fatal error:<br><br>function foo($_GET) {<br>  // whatever<br>}<br><br>It&apos;s called "shadowing" a superglobal, and I don&apos;t know why people ever did it, but I&apos;ve seen it out there. The easy fix is just to rename the variable $get in the function, assuming that name is unique. <br><br>There was no deprecation warning issued in previous versions of PHP, according to my testing, neither in 5.3 nor 5.2. The error messages in 5.4 are:<br>Fatal error: Cannot re-assign auto-global variable _GET in...<br>Fatal error: Cannot re-assign auto-global variable _COOKIE in...<br>etc.  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.superglobals.php)

**[To root](/README.md)**